---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Summer School Series: Lecture 2 by Neil Houlsby"
subtitle: ""
summary: "This lecture was delivered by Neil Houlsby, currently working in Google Brain, Zurich, on Large Scale Visual Representation Learning"
authors: []
tags: [Machine Learning, Computer Vision]
categories: [Google AI Summer School, Experiences]
date: 2020-08-21T23:11:51+05:30
lastmod: 2020-08-21T23:11:51+05:30
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

<a href="https://research.google/people/NeilHoulsby/">Neil Houlsby</a> presented a great talk on Large Scale Visual Representation Learning and how Google has come up with solutions to some classical problems.

### Evaluation of parameters

There are two main ways of evaluating parameters from a network, that extracts the parameters. They are:
* Linear Evaluation: We freeze the weights and retrain the head
* Transfer Evaluation: We retrain end to end with new head

### Visual Task Adaptation Benchmark (VTAB)

<a href="https://ai.googleblog.com/2019/11/the-visual-task-adaptation-benchmark.html">VTAB</a> is an evaluation protocal designed to measure progress towards general and useful visual representations, and consists of a suite of evaluation vision tasks that a learning algorithm must solve. We mainly have three types of tasks, <b> Natural tasks, Specialized tasks and Structured Datasets. </b>

A query that was posed was how useful ImageNet labels would be for pretrained models to work on these three tasks. It has been seen that ImageNet labels work well for Natural images, and not well for the other two tasks.

Representation learners pre-trained on ImageNet can be of three forms:
* GANs and autoencoders
* Self-supervised
* Semi-supervised / Supervised approach

It has been seen that for natural tasks, representations prove to be more important than obtaining more data, and the supervised approach is far better than the unsupervised approach. For structured tasks, a combination of supervised and self-supervised learning works the best.

It was also mentioned that by modern standards, ImageNet is of incredibly small-scale, thus scaling models on ImageNet were not proven to be effective.

Something to specifically keep in mind is that upstream can be expensive, but downstream should be cheap (in terms of both data and compute). For the upstream, examples of suitable large datasets are ImageNet-21k for supervised learning, and YouTube-8M for self-supervised learning.

### BiT-L

Neil introduced the <a href="https://blog.tensorflow.org/2020/05/bigtransfer-bit-state-of-art-transfer-learning-computer-vision.html">Big Transfer Learning (BiT-L)</a> algorithm and talked about it in detail.
The first thing he mentioned about BiT-L was that batch normalization was replaced with <b> group normalization </b> for ultra-large data. Advantages of this were having no train/test discrepancy, and no state which made it easier to co-train with multiple steps.

It was highlighted that optimization at scale implies that schedule is crucial and not obvious. Also, early results of models can be misleading.

To perform cheap tranfer, we need low compute, few/no validation data and diverse tasks. For doing few-shot transfer, pretraining on ImageNet-21k and JFT-300M helps.

#### Robustness

Models trained with ImageNet aren't necessarily robust most of the times. To test OOD robustness (Out-Of-Distribution), we use datasets like ImageNet C, ImageNet R and ObjectNet.

#### Modern Transfer Learning

Modern Transfer Learning calls for a big, labelled datset, a big model and careful training (using about 10 optimization recipes)
While testing with OOD, increasing datset size with a fixed model and increasing dataset size leads to an increase in performance, especially in the case of very large models.

To summarize, Bigger transfer $\rightarrow$ Better Accuracy $\rightarrow$ Better Robustness

* For checking impact on object <b> location </b> invariance, we see accuracy improves and becomes more uniform across location
* This proves to be the same in the case of impact on object <b>size</b> invariance
* However, in the case of object rotation invariance for ResNet50, it does not become more uniform across rotation angles, but for ResNet101*3, it maintains uniformity

### Conclusion
Main takeaways from the talk and BiT-L were:
* Scale is one of the key drivers of representation learning performance
* Especially effective for few-shot learning and OOD Robustness
* Also seen and mirrored in language domain

Links to the GitHub repositories are: <a href ="https://github.com/google-research/big_transfer"> Big Transfer </a> and <a href="https://github.com/google-research/task_adaptation"> Visual Task Adaptation Benchmark (VTAB)</a>




