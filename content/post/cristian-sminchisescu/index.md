---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Summer School Series: Lecture 4 by Cristian Sminchisescu"
subtitle: ""
summary: "This lecture was presented by Cristian Sminchisescu, a professor at Lund University and working at Google Research. The lecture was titled End-to-end Generative 3D Human Shape and Pose models, and active human sensing"
authors: [Archana Swaminathan]
tags: [Computer Vision, 3D Reconstruction]
categories: [Google AI Summer School, Experiences]
date: 2020-08-25T16:11:56+05:30
lastmod: 2020-08-25T16:11:56+05:30
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

This talk was presented by <a href ="https://research.google/people/CristianSminchisescu/">Cristian Sminchisescu</a>, who is a Research Scientist leading a team at Google, and a Professor at Lund University. His talk was titled <b>End-to-end Generative 3D Human Shape and Pose models, and active human sensing</b> 

3D Human Sensing has many applications, in the field of animation, sports motion, AR/VR, medical industry etc. It is a known fact that humans are very complex, the body has 600 muscles, 200 bones and 200 joints. Clothing that humans wear have folds and wrinkles, there are many different types of garments and cloth-body interactions.

### Challenges
Typical challenges in 3D human sensing include:
* High dimensionality, articulation and deformation
* Complex appearance variations, clothing and multiple people
* Self occlusion or occlusion by scene objects
* Observation (depth) uncertainty (especially in monocular images)
* Difficult to obtain accurate supervision of humans

This is where we can exploit the power of machine and deep learning, we aim to come up with a learning model that:
1. Understands large volumes of data
2. Connects between images and 3D models

### Problems that need to be solved

It is imperative to <b>FIND THE PEOPLE </b>. We then need to infer their pose, body shape and clothing. The next step would be to recognize actions, behavioral states and social signals that they make, followed by recognizing what objects they use.

### Visual Human Models

Different Data types we take into consideration are: 
* Multiple Subjects
* Soft Tissue Dynamics
* Clothing
This is all fed into the learning model

### Generative Human Modeling

Dynamic Human Scans $\mathbf{\xrightarrow[\text{deep learning}]{\text{end to end}}}$ Full Body articulated generative human models. The Dynamic Human Scans are in the form of very dense 3D Point Clouds.

### GHUM and GHUML
Cristian then talked about his paper <a href = "https://openaccess.thecvf.com/content_CVPR_2020/papers/Xu_GHUM__GHUML_Generative_3D_Human_Shape_and_Articulated_Pose_CVPR_2020_paper.pdf">GHUM & GHUML: Generative 3D Human Shape and Articulated Pose Models</a>
GHUM is the moderate generative model with 10168 vertices and GHUML is the light version with 3190 vertices, however both have a shared skeleton that has minimal parameterization and anatomical joint limits.

The model faciliates Automatic 3D Landmark detection with multiview renderings, 2D landmark detection and 3D landmark triangulation. Automatic Registration is able to calculate deformations.

### End to End Training Pipeline
{{< figure src="1.png" title="End To End Training Pipeline" numbered="true" lightbox="false"  height=500 width=500 >}}

* Once data is mapped to meshes and put into registered format, next step is to encode and decode static shapes (using VAE)
* Kinematics is learned using Normalizing Flow model
* Mesh filter (mask): to integrate close up scans with models, fed into the optimization step
* To train landmarks, we use annotated image data

### Evaluation
For the variational shape and expression autoencoder, VAE works better than OCA, with reconstruction error lying between 0-20mm. Motion Retargeting and Kinematic Priors are done by retargetting models to 2.8M CMU and 2.2M Humans3.6M motion capture frames.

### Normalizing Flows for Kinematic Priors

{{< figure src="2.png" title="Normalizing Flows for Kinematic Priors" numbered="true" lightbox="false" >}}

* A normalizing flow is a sequence of invertible transformations applied to an original distribution 
* Use a dataset $\mathcal{D}$ of human kinematic poses $\theta$ as statistics for natural human movements 
* Use normalizing flow to warp the distribution of poses into a simple and tractable density function e.g. $\mathbf{z} \sim \mathcal{N}(0 ; \mathbf{I})$ 
* The flow is bijective, trained by maximizing data log-likelihood 
$$\max _{\phi} \sum _{\partial \in \mathcal{D}} \log p _{\phi}(\theta)$$

### GHUM and SMPL

{{< figure src="3.png" title="GHUM vs SMPL" numbered="true" lightbox="false"  height=500 width=500 >}}

* GHUM is close (slightly better) to SMPL in skinning visual quality
* The vertex point-to-plane error (body-only) is GHUM: 4.23mm and SMPL: 4.96mm
### Conclusions
* An effective Deep Learning Pipeline to build generative, articulated 3D human shape models
* GHUM and GHUM are two full body human models that are available for research:(https://github.com/google-research/google-research/tree/master/ghum). 
* We can jointly sample shape, facial expressions (VAEs) and pose (normalizing flows)
* We have low res and high res models, that are non-linear (linear as special case)
### Other Work
Some other interesting papers that Cristian pointed out were <a href="https://arxiv.org/abs/2003.10350"> Weakly Supervised 3D Human Pose and Shape Reconstruction with Normalizing Flows</a> (ECCV 2020) that works on Full Body Reconstruction in Monocular Images, and <a href ="https://arxiv.org/abs/2008.06910">Neural Descent for Visual 3D Human Pose and Shape </a> (submitted to NeurIPS 2020) that talks about Self-Supervised 3D Human Shape and Pose Estimation.

### Human Interactions

A problem that many 3D deep learning practitioners face is dealing with human interactions during estimation and reconstruction. Contacts are difficult to estimate correctly because of:
* Uncertainty in 3D monocular depth prediction
* Reduced evidence of contact due to occlusion

Cristian then talked about his paper <a href="https://openaccess.thecvf.com/content_CVPR_2020/papers/Fieraru_Three-Dimensional_Reconstruction_of_Human_Interactions_CVPR_2020_paper.pdf"> Three-dimensional Reconstruction of Human Interactions </a> and to move towards accurate reconstruction of interactions we need to:
* Detect contact
* Predict contact interaction signatures
* 3D reconstruction under contact constraints
{{< figure src="4.png" title="Modelling interactions" numbered="true" lightbox="false"  >}}


### Conclusion (Interactions)
- New models and datasets for contact detection, contact surface signature prediction, and 3d reconstruction under contact constraints
- Annotation has an underlying contact ground truth but not always easy to precisely identify from a single image
- Humans are reasonably consistent at identifying contacts at 9 and 17 region granularity, and contact can be predicted with reasonable accuracy too
- Contact-constrained 3D human reconstruction produces considerably better and more meaningful estimates, compared to non-contact methods

