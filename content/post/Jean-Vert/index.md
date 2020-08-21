---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Summer School Series: Lecture 1 by Jean-Phillipe Vert"
subtitle: ""
summary: "This lecture was conducted by Jean-Phillipe Vert, a research scientist at Google AI, Paris, on differentiable ranking and sorting"
authors: [Archana Swaminathan]
tags: [Machine Learning, Mathematics]
categories: [Google AI Summer School, Experiences]
date: 2020-08-21T19:07:17+05:30
lastmod: 2020-08-21T19:07:17+05:30
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

This is an article about what <a href ="http://members.cbio.mines-paristech.fr/~jvert/">Jean-Phillipe Vert</a> talked about at the Google Research India-AI Summer School 2020. The lecture was titled <b> Differentiable Ranking and Sorting </b> and lasted about 2 hours.

### Differentiable Programming

What is machine learning and deep learning? 

Machine learning is just to give trained data to a program and get better results for complex problems. For example:

{{< figure src="fig1.png" title="A neural network to recognize cats and dogs" numbered="true" lightbox="false" >}}

These networks usually use <b>vectors</b> to do the computations within the network, however in recent research models are getting extended to non-vector objects (strings, graphs etc.)

Jean then gave an introduction to permutations and rankings and what he aspired to do, informally. Permutations are not vectors/graphs, but something else entirely. Some data are permutations (input, output etc) and some operations may involve ranking (histogram equalization, quantile normalization)

What do these operations aspire to do?
* Rank pixels
* Extract a permutation and assign values to pixels only based on rankings

### Permutations

A permutation is formally defined as a bijection, that is:

$$\sigma:[1, N] \rightarrow[1, N]$$

* Over here, $\sigma(i)=$ rank of item $i$

* The composition property is defined as: $\left(\sigma_{1} \sigma_{2}\right)(i)=\sigma_{1}\left(\sigma_{2}(i)\right)$

* $\mathrm{S}_{N}$ is the symmetric group and 

* $\left|\mathbb{S}_{N}\right|=N !$

### Goal
Our primary goal is:

{{< figure src="2.png" title="Moving between spaces" numbered="true" lightbox="false" >}}

Some definitions here are:
1. Embed:
  * To define/optimize $f_{\theta}(\sigma)=g_{\theta}($embed$(\sigma))$ for $\sigma \in \mathbb{S}_{N}$
  * E.g., $\sigma$ given as input or output

2. Differentiate:
  * To define/optimize $h_{\theta}(x)=f_{\theta}($argsort$(x))$ for $x \in \mathbb{R}^{n}$
  * E.g., normalization layer or rank-based loss

### Argmax

To put it in simple words, the argmax function identifies the dimension in a vector with the largest value. For example, $\operatorname{argmax}(2.1, -0.4, 5.8) = 3$

It is not differentiable because:
* As a function, $\mathbb{R}^{n} \rightarrow[1,n]$, the output space is <b> not continuous </b>
* It is <b>piecewise constant</b> (i.e, gradient = 0 almost everywhere even if the output space was continuous)

### Softmax

It is a <b>differentiable</b> function that maps from $\mathbb{R}^{n} \rightarrow \mathbb{R}^{n}$, where

$$\operatorname{softmax}_ {\epsilon} (x)_ {i} =\frac{e^{x_{i} / \epsilon}}{\sum_{j=1}^{n} e^{x_{j} / \epsilon}}$$

For example, $\operatorname{softmax}(2.1, -0.4, 5.8) = (0.027, 0.02, 0.972)$

### Moving from Softmax to Argmax

$$\lim _ {\epsilon \rightarrow 0} \operatorname{softmax}_{\epsilon}(2.1,-0.4, 5.8)=(0,0,1)=\Psi(3)$$

where $\psi:[1, n] \rightarrow \mathbb{R}^{n}$ is the one-hot encoding. More generally,
$$
\forall x \in \mathbb{R}^{n}, \quad \lim_ {\epsilon \rightarrow 0} \operatorname{softmax}_{\epsilon}(x)=\Psi(\operatorname{argmax}(x))
$$

### Moving from Argmax to Softmax

#### 1. Embedding

Let the simplex
$$
\Delta_{n-1}=\operatorname{conv}(\{\Psi(y): y \in[1, n]\})
$$
Then we have a variational characterization (exercice left to us):
$$
\Psi(\operatorname{argmax}(x))=\underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left(x^{\top} z\right)
$$

{{< figure src="fig3.png" title="Simplex representation" numbered="true" lightbox="false" >}}

#### 2a. Regularization

Let the entropy be defined as $H(z)=-\sum_{i=1}^{n} z_{i} \ln \left(z_{i}\right)$ for $z_{i} \in \Delta_{n-1}$

Then we have (exercise left to us):
$$
\operatorname{softmax}_ {\epsilon}(x)=\underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left[x^{\top} z+\epsilon H(z)\right]
$$

The entropy is maximum at the middle and minimum as the corners, as displayed below

{{< figure src="4.png" title="Entropy in the simplex" numbered="true" lightbox="false" >}}

#### 2b. Pertubation

Let $G=\left(G_{1}, \ldots, G_{n}\right)$ be i.i.d. Gumbel (0,1) random variables. Then we have (exercice):
$$
\operatorname{softmax}_{\epsilon}(x)=E \underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left[x^{\top}(z+\epsilon G)\right]
$$

### Summary

From moving between argmax and softmax, we can:

* Embed, such that
$$
\Psi(\operatorname{argmax}(x))=\underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left(x^{\top} z\right)
$$

* Regularize or pertub:
$$
\operatorname{softmax}_ {\epsilon}(x)=\underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left[x^{\top} z+\epsilon H(z)\right] = E \underset{z \in \Delta_{n-1}}{\operatorname{argmax}}\left[x^{\top}(z+\epsilon G)\right]
$$

Both of these lead to efficient and stochastic Jacobian estimates. We can generalize this to other discrete operations such as rankings, by various techniques, examples being the SUQUAN and Kendall embeddings. 

We then have to make a differentiable approximation to $\Phi (\operatorname{argsort}(x))$, which can be done using <b>Optimal Transport</b> and <b>Entropic Regularization</b>. It has been experimentally proven that this works faster than neural sort for sorting 5 numbers between 0 and 9999.

Jean concluded saying that:
* Machine learning can exist beyond vectors, strings and graphs
* We can calculate different embeddings of symmetric groups
* Differentiable sorting and ranking can be done through regularization and perturbation
* This can be generalized to other discrete operations