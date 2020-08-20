---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Opening Keynote at the Google AI Summer School, 2020"
subtitle: ""
summary: " The opening keynote was delivered by Jeff Dean, Head of Google AI and moderated by Manish Gupta, Director of Google AI Research, Bangalore"
authors: [Archana Swaminathan]
tags: [Google AI Summer School, Machine Learning, Deep Learning]
categories: [Experiences]
date: 2020-08-20T10:53:53+05:30
lastmod: 2020-08-20T10:53:53+05:30
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

This article has been written from notes I took throughout the Opening Keynote at the Google AI Summer School. The opening keynote was delivered by <a href="https://research.google/people/jeff/"> Jeff Dean</a>, Head of Google AI and moderated by <a href="https://research.google/people/106704/">Manish Gupta</a>, Director of Google AI Research, Bangalore. The Keynote was titled <b> Deep Learning to Solve Challenging Problems. </b>

## Introduction

Jeff Dean is a Senior Fellow at Google and the global head of Google AI. He saved Google at a very critical time and is essential to what contributed to make the Google Search Engine the best and the fastest in the world today. He is currently doing exciting research in the field of explainable AI for problems that the world is facing. He also helped create Tensorflow, the world's most used Machine Learning Library.

## Notes from the Talk:

### The marvel of Deep Learning

Deep Learning has revolutionized the way of solving challenging problems. There are over 130 new papers on Machine Learning on Arxiv every day. Deep Learning can be considered a modern reincarnation of Artificial Neural Networks. Key benefits and features of Deep Learning are:

* Availability of new network architectures
* Ability to scale to larger datasets and efficient computation of the math
* Learns features from raw, noisy, heterogenous data
* No explicit feature engineering required

Deep Learning architectures are remarkably flexible with taking in inputs and giving outputs of various forms, some examples are getting a categorical label from a pixel input (image), an audio input translating to a phrase that is a string, and language translation from one language to another.

Deep Learning has also helped us come up with solutions to problems where the computer can achieve better results than a human. One such example is the <a href="http://www.image-net.org/challenges/LSVRC/">Imagenet challenge</a>, that Stanford conducts every year that classifies images into classes.

* In 2011, the winner of the challenge was able to achieve 26% error, where humans were able to do the same task with 5% error.
* In 2012, <a href= "https://scholar.google.co.uk/citations?hl=en&user=JicYPdAAAAAJ">Geoffrey Hinton</a> and his team used Deep Learning for the very first time in this challenge, and was the pioneer of bringing deep convolutional networks for the image classification task. Following his attempt, Deep Learning became very popular in further editions of the challenge.
* In 2017, the winner of the challenge was able to achieve 3% error on the Imagenet dataset, finally beating the human error of 5%.

### Deep Learning to solve world problems
One thing that Jeff emphasized on, is how Deep Learning is being used to tackle the <a href="http://www.engineeringchallenges.org/challenges.aspx">Grand Engineering Challenges of the 21st century</a>
One of the primary challenges that are under focus are restoring and improving urban infrastructure.

A key advancement in this field is autonomous driving, which Deep Learning has aided to such an extent that the autonomous driving is far safer than the usual human driver, with 360-degree vision utilizing around 18 cameras to form a dense LiDAR point cloud. 

Another field that Deep Learning revolutionalized is combining vision with robotics. For the task of a robot arm picking up an unseen object,
* In 2015, there was a 65% grasp success rate
* In 2016, with the robot trained to pick up multiple categories of objects, the accuracy rose up to 78%
* In 2018, this accuracy shot up to 96% when Deep Learning was introduced into the mix
Self supervised imitation learning also uses deep learning, which is the ability of a robot to imitate actions from pixels (human footage) without supervision

Another of these challenges was Advancing Health Informatics. We got an insight into what Google AI is working on for this field. 
* One of these is diagnosing diabetic retinopathy, the fastest growing cause of preventable blindness
* Screening of the individual can prevent blindness, however it is extremely specialized so most MD's cannot do it. 
* Google came up with a <a href="https://ai.googleblog.com/2018/12/improving-effectiveness-of-diabetic.html">model</a> that could diagnose the disease from image scans of the eye, in 2016 it was at par with the performance of general opthamologists, and in 2017 the accuracy became State of the Art, with accuracy matching that of Retinal Specialists

Many of these challenges and advances in the field of engineering and technology depend on the ability to understand text. The 2017 Tranformer Paper: <a href="https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf"> Attention is all you need!</a> was a revolutionary step in this direction, which was followed by <a href="https://arxiv.org/abs/1810.04805">BERT</a> in 2018. BERT introduced principles for training that was very popular and appreciated, that are:
* Pre train a model on the "fill in the blanks" task, using large amounts of self supervised text.
* This model is then fine tuned on individual language tasks, on a smaller scale.

This brought in light the desire to have large model architectures that are sparsely activated, that desirably have huge remembering capacity but utilize only a small fraction of the model while testing with individual examples
An example of this is the Per-Example Routing architecture.

Jeff highlighted one of the most major contributions from Google towards deep learning, the introduction of the open-source deep learning library <a href="https://www.tensorflow.org/">Tensorflow</a>. It remains the most popular and most downloaded Deep Learning Library until date, and has a vibrant open-source community, to the extent that only 1/3<sup>rd</sup> of the current contributors are employees of Google!

### Computer architecture for Deep Learning

There was a time in the past where complex problems couldn't be solved because of the lack of computational power. We have finally made strides that do not restrict the power available to us, so optimizing this is an important task. 
Google AI has been focusing on redesigning computers, as Deep Learning has transformed this field completely. They kept two main things in mind while devising a computer to do deep learning:
* First is, reduced precision is okay. The computer does not have to calculate results acccurately to the 10<sup>th</sup> or 20<sup>th</sup> decimal point. 
* Second is, there are mostly only a handful of specific operations that constitute the math of Deep Learning, for example matrix multiplication, dot products, etc.

Keeping these in mind, Google introduced the <a href="https://cloud.google.com/tpu/docs/tpus">Tensor Processing Unit</a>, that does just this. We can connect TPUs together to form Pods, that are currently available to the public on cloud services. Pods can be connected together to make supercomputers, that can train architectures like ResNet50 and Inceptionv2 in under 30 seconds! TPUs are being designed for edge applications also, to do deep learning on smartphones.

### Problems of doing Machine Learning Today

The usual flow of work for a machine learning specialist is to collect the data, use his ML expertise (data augmentation, hyperparameter tuning etc) and train and test the model. A rather new approach that reduces human intervention here is <a href="https://en.wikipedia.org/wiki/Automated_machine_learning">AutoML</a>, where the "ML expertise" is tuned and tested by automatic methods.

Problems that still remain are:
* We still start with little to no knowledge about the problem and have to rely on random initialization
* New problems need significant data and compute power
* Transfer learning and multi-task learning help with this, but are done modestly

### What is desired to be achieved

* Large but sparsely activated neural network architectures
* A single model that can be used to solve many tasks, by activating different parts of the network
* Dynamically adapting to new problems
* Adding new tasks easily


Thus concluded the Keynote. It was fantastic and insightful. A couple of Q&A that I found interesting have been mentioned below:

Q: What advice do you have for young researchers?

A: Focus on problems that matter to you, and learn as much as you can. Create a constellation of techniques and ideas that can help you gather and organize your thoughts

Q: How do you read new papers and get a gist of it?

A: You'll find many discussions on LinkedIn and Twitter about the paper, sometimes just reading this will give you a gist of what's going on in the paper

Q: Something unrealistic that you wish would happen in the field of AI in the future?

A: The creation of a system that can absorb the world's knowledge and solve all our problems

Q: Hyperparameter tuning is expensive for large models, how do researchers work on this?

A: Scaling down the problem to probably 1% of it and training and tuning that completely, and marginal scaling up to the level you desire is the best approach


