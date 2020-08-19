---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Understanding Deep Learning requires rethinking generalization"
subtitle: ""
summary: "A short and concise review of the ICLR paper"
authors: [Archana Swaminathan]
tags: [Machine Learning]
categories: [Reviews]
date: 2020-07-18T14:11:22+05:30
lastmod: 2020-07-18T14:11:22+05:30
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
This is a review of the ICLR 2017 paper by Zhang et. al. titled "Understanding Deep Learning requires rethinking generalization"<a href = "https://bengio.abracadoudou.com/cv/publications/pdf/zhang_2017_iclr.pdf"> Link to paper </a>


The paper starts off with aiming to provide an introspection into what distinguishes networks that generalize well, from those who don’t. 

One of the experiments conducted in the studies of the paper, is checking how well neural networks adapt to training when labels are randomized. Their findings establish that when the true data is completely randomly labelled, the training error that results is zero. Observations from this indicate that the effective capacity of neural networks is more than enough to memorize the entire dataset. Randomization of the labels is only a transformation of the data, and other learning parameters are constant and unchanged still. The resulting training time also increases by only a small factor. However, when this trained network is tested, it does badly. This indicates that just by randomizing labels, the generalization error can shoot up significantly without changing any other parameters of the experiment like the size of the model, the optimizer etc.

Another experiment conducted was that when the ground truth images were switched with random noise. This resulted in the networks training to zero training error, even faster than the case with the random labels. Varying the amount of randomization resulted in a steady deterioration of the generalization error, as the noise level increased. There were a wide variety of changes introduced into the dataset, that played with degrees and kinds of randomization with the pixels and labels. All of this still resulted in the networks able to fit the training data perfectly. A key takeaway from this is that the neural networks are able to capture the signals remaining in the data, while fitting the noise and randomization with brute force. The question that still remains unanswered after this is why some models generalize better than others, because it is evident that some decisions made while constructing model architectures do make a difference in its ability to generalize.

Traditional approaches in statistical learning theory such as Rademacher complexity, VC dimension and uniform stability are threatened by the randomization experiments performed. 

Three specific regularizers are then considered to note the impact of explicit regularization, data augmentation, weight decay and dropout. These are tried out on Inception, Alexnet and MLPs on the CIFAR10 dataset, and later with ImageNet. Regularization helps to improve generalization performance, but the models still generalize well enough with the regularizers turned off. It was then inferred that this is more of a tuning parameter than a fundamental cause of good generalization. A similar result was noted with implicit regularization.

An interesting result proved in the paper was that there two layer depth networks of linear size, that can represent any labelling of the training data. A parallel approach in trying to understand the source of regularization for linear models was also not easy to point out. 
To sum up, this paper presents a thorough insight into how empirically easy optimization does not imply good regularization, and effective capacity of network architectures is better understood and defined.
