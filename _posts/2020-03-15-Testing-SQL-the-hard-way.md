---
layout: post
title: Testing SQL the hard way
description: Analytics tables are easy to get wrong; finding out that they are wrong is even harder.
categories: [data, monzo]
---

### 👯‍♂️ A table of users
Let's look at a toy example. I've written all of this in BigQuery SQL, so that you can copy/paste it straight into the [console](https://console.cloud.google.com/bigquery) and play with it yourself.

Imagine you have a JSON payload (an "event") each time a user creates an account, with the `user_id` and the timestamp of when the account was created. Here's some mock data:

```sql
WITH account_created_events AS (
 SELECT *
 FROM UNNEST([
  "{\"user_id\": \"user_1\", \"account_created\": \"2020-01-01\"}",
  "{\"user_id\": \"user_2\", \"account_created\": \"2020-01-02\"}",
  "{\"user_id\": \"user_3\", \"account_created\": \"2020-01-03\"}",
  "{\"user_id\": \"user_4\", \"account_created\": \"2020-01-04\"}"
  ]) AS event
),
```

When those users finish some set of actions (e.g., completing their profile), we get another event to tell us that the user has completed their signup. Here's some more mock data, for those same users:

```sql
signup_completed_events AS (
 SELECT *
 FROM UNNEST([
  "{\"user_id\": \"user_1\", \"signup_completed\": \"2020-01-02\"}",
  "{\"user_id\": \"user_2\", \"signup_completed\": \"2020-01-03\"}",
  "{\"user_id\": \"user_3\", \"signup_completed\": \"2020-01-04\"}",
  "{\"user_id\": \"user_4\", \"signup_completed\": \"2020-01-05\"}"
  ]) AS event
),
```

We can take those two events and create a **users** table, with one row per user:

```sql
CREATE OR REPLACE TABLE users AS 

-- Extract the account creation columns
WITH accounts_created AS (
  SELECT 
  JSON_EXTRACT_SCALAR(event, "$.user_id") AS user_id,
  TIMESTAMP(JSON_EXTRACT_SCALAR(event, "$.account_created")) AS account_created

  FROM account_created_events
),

-- Extract the signup completion columns
signups_completed AS (
  SELECT 
  JSON_EXTRACT_SCALAR(event, "$.user_id") AS user_id,
  TIMESTAMP(JSON_EXTRACT_SCALAR(event, "$.signup_completed")) AS signup_completed

  FROM signup_completed_events
)

-- Join them together
SELECT
user_id,
accounts_created,
signups_completed,
TIMESTAMP_DIFF(s.signup_completed, a.account_created, HOUR) AS signup_time

FROM accounts_created
LEFT JOIN signups_completed USING (user_id)
```

And here are the results:

| Row | user_id | account_created | signup_completed | signup_time |
|---|---|---|---|---| -- |
| 1 | user_1 | 2020-01-01 00:00:00 UTC | 2020-01-02 00:00:00 UTC | 24 |
| 2 | user_2 | 2020-01-02 00:00:00 UTC | 2020-01-03 00:00:00 UTC | 24 |
| 3 | user_3 | 2020-01-03 00:00:00 UTC | 2020-01-04 00:00:00 UTC | 24 |
| 4 | user_4 | 2020-01-04 00:00:00 UTC | 2020-01-05 00:00:00 UTC | 24 |

Perfect! This is the ideal analytics table to answer a ton of different basic analytic questions:
- How many users do we have?
- How many of them signed up today?
- How long does it take users, on average, to complete signup?

This is also the type of table that could be extended with tens of columns, each representing unique pieces of information about each user. As long as this table keeps its core assumption (one row per user), we'll be good to go. How could this go wrong?

### 🙃 Someone completes the signup flow twice

Let's imagine that a customer, due to whatever reason, completes the signup flow twice. Maybe there's a bug in the app and the customer uninstalls & reinstalls the app, maybe there was a bug or failure in the backend service that is emitting those events (whatever, it doesn't matter!).

Nothing has changed in the analytics code base, but now we have two signup completed events for that user:

```sql
signup_completed_events AS (
 SELECT *
 FROM UNNEST([
  "{\"user_id\": \"user_1\", \"signup_completed\": \"2020-01-02\"}",
  "{\"user_id\": \"user_2\", \"signup_completed\": \"2020-01-03\"}",
  "{\"user_id\": \"user_3\", \"signup_completed\": \"2020-01-04\"}",
  "{\"user_id\": \"user_4\", \"signup_completed\": \"2020-01-05\"}",
  "{\"user_id\": \"user_3\", \"signup_completed\": \"2020-02-04\"}" -- Yikes!
  ]) AS event
),
```

This tiniest of errors -- something that is largely invisible to the person who is _writing_ the SQL, makes the result table have an off by one error: `user_3` appears twice. Not only that, but your stats on signup completion rates have shot through the roof!

| Row | user_id | account_created | signup_completed | signup_time |
|---|---|---|---|---|--|
| 1 | user_1 | 2020-01-01 00:00:00 UTC | 2020-01-02 00:00:00 UTC | 24 |
| 2 | user_2 | 2020-01-02 00:00:00 UTC | 2020-01-03 00:00:00 UTC | 24 |
| 3 | user_3 | 2020-01-03 00:00:00 UTC | 2020-01-04 00:00:00 UTC | 24 |
| **4** | **user_3** | **2020-01-03 00:00:00 UTC** | **2020-02-04 00:00:00 UTC** | **768** |
| 5 | user_4 | 2020-01-04 00:00:00 UTC | 2020-01-05 00:00:00 UTC | 24 |

Herein lies the problem: a tiny issue in the data has propagated itself through the `LEFT JOIN`s in the analytics code base, and has completely skewed some metrics that you are using. This gets even more complex if this table has many 10s of columns and/or if the output table is used by downstream queries.

These problems are, to say the least, a huge headache to diagnose and fix. Indeed, they may first manifest as a question about the _metrics_ ("why has our average sign up time gone through the roof?") and not about the _data_. 

### 🐛 "Unit" testing a table in two steps

There are many different things that you can do to remedy the problem above. This post is not about fixing those bugs: it is only about an approach that we used to use to _detect_ these types of problems with some sort of meaningful error message.

At its core, this method relies on **enumerating your assumptions about the structure of the table**. In our example, our key assumption was that the table has **one row per user**. 

**Step 1**. Write a query that should return an empty result if your table's assumptions are not broken:

```sql
CREATE OR REPLACE TABLE users_table_errors AS 

WITH user_validation AS (
  SELECT
  user_id,
  COUNT(*) AS num_rows

  FROM users
  GROUP BY 1
  HAVING num_rows > 1 -- Should never be the case, amirite?
)

-- Maybe there is more than one validation (e.g. validating time stamps)

SELECT
"Duplicated user_ids" AS failure_type,
COUNT(DISTINCT user_id) AS num_failures

FROM user_validation
```

These types of queries are an extremely useful way of (a) documenting what you expect, and (b) creating a table of all of the (in this case) user ids that don't match your expectations.

**Step 2**. If a validations table is _not_ empty, we used the `ERROR()` function in BigQuery to stop the query! It looks like this:

```sql
SELECT
ERROR(CONCAT("ERROR: ", num_failures, " ", failure_type)) AS error

FROM users_table_errors
WHERE num_failures != 0
```

### 🏚 Why is this the "hard way" of testing?

