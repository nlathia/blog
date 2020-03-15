---
layout: post
title: Deep learning & medical diagnosis
description: A growing list of areas where machine learning is being applied for diagnosis based on medical imaging.
categories: [machine-learning, health]
---

Over the last few months, there have been a number of announcements of research findings that claim that deep learning has been applied to, and often times immediately outperforms doctors in, a particular area of diagnosis.

I originally started this blog post to keep track of them — I’m going to publish it as a draft that I expect to update on a regular basis.

## What is deep learning in medical image diagnosis trying to do?
Before diving into the specific results, I’d like to highlight that the approaches (so far) below share the same common pattern. I’ve tried to summarize this into the following sentence:

> Diagnosis via machine learning works when the condition can be reduced to a classification task on physiological data, in areas where we currently rely on the clinician to be able to visually identify patterns that indicate the presence or type of the condition.

To break this down into details:
- **Classification**. The results are areas of medical diagnosis that can be reduced to a [classification problem](https://en.wikipedia.org/wiki/Statistical_classification): given some data, diagnosis can be reduced to the problem of mapping that data to one of N different outcomes. In some cases, N = 2: the task is just to identify whether the data (e.g., an x-ray) displays the condition or not. Note that there are other problems (e.g., image segmentation) that can be tackled with deep learning, but I haven’t yet seen them used in isolation for diagnosis (as opposed to, say, analysis only).
- **Physiological data**. The results below tend to be using medical imaging data or data from other kinds of sensors. The explosion of results in these areas is, in large part, ascribed to the creation of data sets (e.g., [these ones](http://www.radrounds.com/profiles/blogs/list-of-open-access-medical-imaging-datasets)) that are far larger than those that were previously available. A common approach to annotate a data set (e.g., label whether an x-ray contains a tumor) is to have a set of clinicians give their opinion and to collate the responses.
- **We rely on visually identifying patterns**. The alternative to an automated diagnosis system would be to have an expert clinician look at your data (perhaps discuss it with some fellow experts) to determine the outcome. This point captures why deep learning should be successful in this area: deep learning automates the entire process of extracting patterns and learning relationships in this kind of ‘unstructured’ data. There are many non-medical applications of deep learning (e.g., [face recognition](https://en.wikipedia.org/wiki/DeepFace)) that have similar requirements; because of this, the tech is quite mature. Indeed, even models trained on medical images are now being [open sourced](https://github.com/DLTK/models).

For great reviews of the area, check out the review papers and blog posts that I’ve added in the references section at the bottom of this post.

## Where should this approach not work?

The pattern above gives some insight into areas where this approach should not currently work. Like [this article mentions](https://lukeoakdenrayner.wordpress.com/2016/11/27/do-computers-already-outperform-doctors/), deep learning does not tell us how patients should be treated or how well they will fare when being treated. However, specifically related to the points above, there are domains that:
- Aren’t classification problems. If we don’t understand the disease well enough, we can’t create data to train any algorithm. For example, there are conditions that do not have a well-understood progression that can be enumerated into a set of stages. In these cases, it would be very challenging to build a reliable model to tell us what stage of progression a patient is at — because we don’t know what the stages should be.
- Lack (or have subjective) data. If there is little or no data, we cannot train models. Granted, this is starting to change — there are deep learning experiments that demonstrate [learning from extremely small data sets](https://medium.com/@radekosmulski/can-we-beat-the-state-of-the-art-from-2013-with-only-0-046-of-training-examples-yes-we-can-18be24b8615f). Where there is data, but it and/or patterns within it are subjective (e.g., the momentary experience of pain or stress), then I’d imagine that the approaches below would need to be re-imagined.
- Do not rely on medical devices. Similarly, domains where a diagnosis cannot be derived by attaching the patient to some kind of machine and collecting a single “sample” of data (e.g., requiring long-term tracking or [diagnosis by exclusion](https://en.wikipedia.org/wiki/Diagnosis_of_exclusion)). This could be because (a) we have not developed a means of detecting a disease — so, like above, more fundamental research is required, or (b) we have not developed viable products for long-term, non-invasive monitoring to collect the data that enables machine learning.

## List of Medical Conditions

My criteria for adding to this list: (a) a data set has been released, (b) research has been published,(c) a company or research group has written about work in progress, or (d) there are blog posts that describe tackling the problem. I’ve sorted the conditions alphabetically.

Have I missed something? You can [@ me on twitter](https://twitter.com/neal_lathia) and I’ll add it.

### Alzheimer’s

… “is a [chronic neurodegenerative](https://en.wikipedia.org/wiki/Alzheimer%27s_disease) disease that usually starts slowly and worsens over time.” Researchers in London have [published a paper](https://arxiv.org/abs/1502.02506) that reports using data from [the ADNI](http://www.adni-info.org/Scientists/ADNIData.html) to train a 3 layer neural network with a single convolutional layer that can predict whether an [MRI scan](https://en.wikipedia.org/wiki/Magnetic_resonance_imaging) is a healthy brain, a brain with mild cognitive impairment, and a brain with Alzheimer’s disease.

### Arrhythmia

… “is a [group of conditions](https://en.wikipedia.org/wiki/Heart_arrhythmia) in which the heartbeat is irregular.” Researchers at Stanford have [published a paper](https://arxiv.org/abs/1707.01836) that reports that a 34 layer convolutional neural network they developed “exceeds the performance of board certified cardiologists in detecting a wide range of heart arrhythmias from electrocardiograms recorded with a single-lead wearable monitor” ([project page](https://stanfordmlgroup.github.io/projects/ecg/), [blog post](https://blog.acolyer.org/2017/08/14/cardiologist-level-arrhythmia-detection-with-convolutional-neural-networks/)).


### Autism

… “is a [neurodevelopmental disorder](https://en.wikipedia.org/wiki/Autism) characterized by impaired social interaction.” A team of researchers [published a paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5336143/), which reports that “a deep learning algorithm primarily using surface area information from brain MRI at 6 and 12 months of age predicted the 24 month diagnosis of autism in children at high familial risk for autism”(via [@datarequena](https://twitter.com/datarequena/status/938437786037825536) on twitter).

### Breast Cancer

… “is a cancer that [develops from breast tissue](https://en.wikipedia.org/wiki/Breast_cancer).” [DeepMind Health](https://deepmind.com/applied/deepmind-health/) has published [a blog post](https://deepmind.com/blog/applying-machine-learning-mammography/) where they announced that they have teamed up with Cancer Research UK to analyse and apply machine learning on anonymised [mammograms](https://en.wikipedia.org/wiki/Mammography) from 7,500 women.

### Dental Cavities

… “is a [break down of teeth](https://en.wikipedia.org/wiki/Tooth_decay) due to acids made by bacteria.” Researchers at [ParallelDots](https://paralleldots.xyz/) have [published a paper](https://arxiv.org/abs/1711.07312v2) that reports that a 100+ layer convolutional network that performs pixel-level binary classification of teeth radiographs (has caries/does not have caries).

### Diabetic Retinopathy

… “is a [medical condition](https://en.wikipedia.org/wiki/Diabetic_retinopathy) in which damage occurs to the retina due to diabetes.” Over two years ago, there was a [kaggle competition](https://www.kaggle.com/c/diabetic-retinopathy-detection) that sought to classify images of eyes into one of 5 classes (from no diabetic retinopathy, to mild, moderate, severe, and proliferative). The winning solution used a combination of [sparse convolutional networks](https://github.com/facebookresearch/SparseConvNet) and a random forest to make a prediction from a pair of images (left and right eye) to the outcome.

### Gram Stains

… are a “[method](https://en.wikipedia.org/wiki/Gram_stain) of [staining](https://en.wikipedia.org/wiki/Staining) used to distinguish and classify [bacterial](https://en.wikipedia.org/wiki/Bacteria) species into two large groups.” It is a laboratory technique that is performed on bodily fluids (e.g., blood) when an infection is suspected. Researchers have [published a paper](http://jcm.asm.org/content/early/2017/11/24/JCM.01521-17) (cited in [this article](https://www.digitaltrends.com/cool-tech/microscope-blood-infections-ai/)) that describes using a convolutional neural network to classify microscope images into [gram-positive](https://en.wikipedia.org/wiki/Gram-positive_bacteria), [gram-negative](https://en.wikipedia.org/wiki/Gram-negative_bacteria), and background (no cells). In the paper, they describe that they did not train a CNN from scratch; they fine-tuned [Inception v3](https://arxiv.org/abs/1512.00567) for this task.

### Lung Cancer

… “is a [malignant tumor](https://en.wikipedia.org/wiki/Lung_cancer) characterized by uncontrolled cell growth in tissues of the lung.” This [2017 kaggle competition](https://www.kaggle.com/c/data-science-bowl-2017#description) included a data set of [CT scans](https://en.wikipedia.org/wiki/CT_scan), and the goal was to predict the likelihood of lung cancer. There were some interesting challenges here, including the fact that the data is in 3-dimensions — the write-ups of [winning solutions](http://blog.kaggle.com/2017/06/29/2017-data-science-bowl-predicting-lung-cancer-2nd-place-solution-write-up-daniel-hammack-and-julian-de-wit/) describe some interesting ways to tackle this. This [blog post](https://medium.com/@alexandrecadrin/lung-cancer-bridging-the-gap-between-medical-imaging-and-data-science-a92b0bb08fda) outlines some of the limitations of this competition, from a clinical perspective. Separately, it looks like [Enlitic is also working on](https://www.enlitic.com/press-release-11162016.html) a lung cancer screening solution.

### Onychomycosis

… “is a [fungal infection of the nail](https://en.wikipedia.org/wiki/Onychomycosis).” As [this tweet](https://twitter.com/DrLukeOR/status/954670235704901632) pointed out, researchers in Korea have [published a paper](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0191493#pone-0191493-t001) that reports using CNNs (VGG-19, ResNet-152) to both create a training dataset (e.g., extracting hand and foot images from a clinical photographs) as well as classify nails into one of six classes (as follows: (onychomycosis, nail dystrophy, onycholysis, melanonychia, normal, and others) to achieve “a diagnostic accuracy for onychomycosis using deep learning that was superior to that of most of the dermatologists who participated in this study.”

### Pneumonia

… “is an [inflammatory condition](https://en.wikipedia.org/wiki/Pneumonia) of the lung affecting primarily the small air sacs known as alveoli.” Researchers at Stanford have [published a paper](https://arxiv.org/abs/1711.05225) that reports that a 121 layer convolutional neural network they developed “can detect pneumonia from chest X-rays at a level exceeding practicing radiologists” ([project page](https://stanfordmlgroup.github.io/projects/chexnet/)).

### Skin Cancer

… “are due to the development of [abnormal cells that have the ability to invade or spread to other parts of the body](https://en.wikipedia.org/wiki/Skin_cancer).” Researchers at Stanford have published a paper that reports fine-tuning [Inception v3](https://www.tensorflow.org/tutorials/image_recognition) to classify 757 disease classes, using a “dermatologist-labelled dataset of 129,450 clinical images, including 3,374 dermoscopy images.” The results are examined across different prediction tasks and accuracy rates seem comparable to the clinicians’ scores.

…I’m sure that this list will grow.

## References & Resources

- [1702.05747](https://arxiv.org/abs/1702.05747) A Survey on Deep Learning in Medical Image Analysis
- [Medium](https://medium.com/the-mission/up-to-speed-on-deep-learning-in-medical-imaging-7ff1e91f6d71) Up to Speed on Deep Learning in Medical Imaging
- [NCBI](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5447633/) Deep Learning in Medical Imaging: General Overview
- [Science direct](http://www.sciencedirect.com/science/article/pii/S1361841517301135) A survey on deep learning in medical image analysis
- [Github](https://github.com/albarqouni/Deep-Learning-for-Medical-Applications) Deep-Learning-for-Medical-Applications
