---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Summer School Series: Lecture 6 by Arsha Nagrani"
subtitle: ""
summary: "This lecture was delivered by Arsha Nagrani, a recent Ph.D. graduate from Oxford University's VGG group, and an incoming research scientist at Google Research, titled Multimodality for Video Understanding"
authors: [Archana Swaminathan]
tags: [Computer Vision]
categories: [Google AI Summer School, Experiences]
date: 2020-08-25T18:36:00+05:30
lastmod: 2020-08-25T18:36:00+05:30
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
This final lecture was delivered by <a href ="http://www.robots.ox.ac.uk/~arsha/">Arsha Nagrani</a>, a recent Ph.D. graduate from Oxford University's VGG group, and an incoming research scientist at Google Research. Her talk was called <b>Multimodality for Video Understanding</b>.

### Video Understanding

Videos provide us with far more information than images. Multimodal refers to many mediums for learning, here it can be time, sound and speech. Videos are all around us (30k newly created content videos are uploaded to YouTube every <b>hour</b>).
However, these have high dimensionality and are difficult to process and annotate.

#### Complementarity among signals

* Vision (scene)
* Sound (content of speech)

#### Redundancy between signals
Helps recognize person, face+sound, thus can be a useful form of weak supervision. The redundant information comes from background sounds, foreground audio, signals identified from speech and the content of speech. 

Thus, best way to exploit multimodal nature of videos is to work with the complementarity and redundancy.

#### Suitable tasks 
Suitable tasks for video understanding are:
1. Video classification
  - single label
  - infinite number of possible classes
  - ambiguity in the label space

2. Action recognition: more fine grained, the motion is important, human centric

It is important to note that labelling actions in videos is extremely expensive and existing models do not generalize well to new domains.

In this context, can we use speech as a form of supervision? For example, narrated video clips and lifestyle Vlogs.

### Movies

General domain of movies: people speak about their actions. However, sometimes speech is completely unrelated, giving us noise. We need to learn when speech matches action. An example of work in this field is <a href ="https://arxiv.org/abs/1912.06430">End-to-End Learning of Visual Representations from Uncurated Instructional Videos</a>. This work reduces noise by using the MIL-NCE loss.

Can we first train a model to recognize actions and then see if it should be used for supervision? An interesting discovery Arsha made was using Movie Screenplays, that contain both speech segments and scene directions with actions. Using this:
* We can obtain speech-action pairs
* Retrieve speech segments with verbs
* Train the <a href="https://www.robots.ox.ac.uk/~vgg/research/speech2action/">Speech2Action</a> model to predict action, with a BERT-Backbone (movie scripts scraped from IMSDB)
* Apply to closed captions of unlabelled videos
* Apply to large movie corpus

 {{< figure src="1.png" title="Speech2Action model" numbered="true" lightbox="false" >}}

The Speech2Action model recognizes rare actions, and is a visual classifier on weakly labelled data (S3D-G model with cross-entropy loss)

Evaluation is done on the AVA and HMDB-51 (transfer learning) datasets. It gets abstract actions like <b>count</b> and <b>follow</b> too.

### Multimodal Complementarity

This refers to fusing info from multiple modalities for video text retrieval, like:
- Finding video corresponding to text queries
- More to videos than just actions like object, scene etc.

Supervisions:
- It's not easy to get the complete combination of captions, this is a very subjective task
- Need extremely large datasets

What Arsha does is rely on expert models trained for different tasks like object detection, face detection, action recognition, OCR etc. These are all applied to the video and features are extracted. The framework is a joint video text embedding, with the video encoder + text query encoder = joint embedding space (similarity should be really high if related). It is necessary for the video encoder to be discriminative and retain specific information.

### Collaborative Gating

For each expert, generate attention mask by looking at the other experts <a href = "https://bmvc2019.org/wp-content/uploads/papers/0363-paper.pdf"> (Use What You Have: Video Retrieval Using Representations From Collaborative Experts, BMVC 2019)</a>
- Trained using bi-directional max margin ranking loss
- Adding in more experts massively increases performance
- Main boost is from the object embeddings

 {{< figure src="2.png" title="Collaborative Gating" numbered="true" lightbox="false" >}}

 Another paper that Arsha discussed was <a href ="https://arxiv.org/abs/2007.10639"> Multi-modal Transformer for Video Retrieval, ECCV 2020 </a>. This takes features that are taken at different time stamps for each task and aggregrate for the embeddings. The expert and temporal embeddings are added and summed up.

 ### Conclusion

 * More modalities is better (because more complementarity)
 * Time (modelling time along with modalities is interesting, some modalities train faster than the others)
 * Mid fusion is better than late (Attention truly is what you need)
 * Our world is multimodal, it doesn't make sense to work with modalities in isolation
 * Use the redundant and complementary information from vision, audio and speech to massively reduce annotations

 <b>Open Research Questions:</b>
 1. Extended Temporal Sequences (beyond 10s): 
  - Backprop + memory restricts current video architectures to 64 frames
  - For longer we rely on pre-extracted features
  - Need new datasets to drive innovation
 2. Moving away from supervision: is an upper bound on self supervision being appraoched?
 3. The world is multimodal: how do we design good fusion architectures?


 Arsha thus concluded a fantastic talk that described the cutting-edge research that her team at Oxford and Google is conducting. It was tremendously insightful and inspirational. 