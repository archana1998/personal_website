---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/
markup: mmark
title: "Compressive Sensing and Denoising of Images using the Ramanujan Fourier Transform"
summary: "Used the Ramanujan Fourier Transform to do compressive sensing and denoising of images in the Ramanujan domain,using the Ramanujan basis as the overcomplete dictionary and trained the dictionary with K-SVD based on OMP algorithm"
authors: [Archana Swaminathan]
tags: [Image Processing]
categories: [Undergraduate Projects]
date: 2020-07-18T18:40:47+05:30

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

## Introduction
The Ramanujan Sums were first proposed by Srinivasan Ramanujan in 1918, and have become exceedingly popular in the fields of signal processing,time-frequency analysis and shape recognition. The sums are by nature, orthogonal. This results in them offering excellent conservation of energy, which is a property shared by Fourier Transform as well. 

We have used Matrix Multiplication to obtain the Ramanujan Basis, for our computation. The Ramanujan Sums are defined as n<sup>th</sup> powers of q<sup>th</sup>primitive roots of unity, which can be computed using this simple formula:


$$c_{q}(n)=\mu\left(\frac{q}{g c d(q, n)}\right) \frac{\varphi(q)}{\varphi\left(\frac{q}{g c d(q, n)}\right)}$$

Where $q=\prod_{i} q_{i}^{a_{i}}$,(q<sub>i</sub> is prime). Then, $\varphi(q)=q \prod_{i}\left(1-\frac{1}{q i}\right)$.

$\mu(n)$ is the Mobius function, which is equal to 0 if n contains a square number, 1 if n = 1 and (-1)*k if n is a product of k distinct prime numbers.

The Ramanujan matrix can be defined as:

$$A(q, j)=\frac{1}{\varphi(q) M} c_{q}(\bmod (j-1, q)+1)$$

The 2-D forward Ramanujan Sum Transform is given as:

$$Y(p, q)=\frac{1}{\varphi(p) \varphi(q)} \frac{1}{M N} \sum_{m=1}^{M} \sum_{n=1}^{N} x(m, n) C_{p}(m) C_{q}(n)$$

which in matrix terms can be defined as

$$Y=A * A^{\top}$$

and the inverse 2D Ramanujan transform in matrix terms is:

$$X=A^{-1} Y\left(A^{-1}\right)^{\top}$$

{{< figure src="montage1.jpg" title="Original, Transformed and Inversed Image" numbered="true" lightbox="false" >}}

## Compressive Sensing

The principle behind the use of compressive sensing as a signal processing technique, is that most test signals are not actually completely comprised of noise, but most have a great degree of redundancy in them. Sparse representation of signals in a particular domain signifies that most of the signal coefficients are either zero or close to zero.

Compressive measurements, which are a weighed linear combination of signal samples, are first taken in a basis that is different from the sparse basis. 

### Algorithm

We use the generated Ramanujan Basis to do the sparse reconstruction. First, the sparse signal is obtained by multiplyingthe Ramanujan Basis A with the flattened image vector (here we are using the Cameraman Image that has been resized to 50*50).
Our Ramanujan Basis has dimensions of 2500*2500. 

Thus, 

$$Z=A^{*} x$$

Where Z is the sparse representation of the cameraman image, and x is the flattened image vector of the original image. 
We next create a random measurement matrix of dimension m*n, where we keep m = 5000 and n = 2500. This measurement matrix (Phi) is then multiplied by the sparse signal z.

$$Y=P h i * Z$$

We then use Orthogonal Matching Pursuit Algorithm, which aims to approximately find the most accurate  projections of data in multiple dimensions on to the span of a redundant or overcomplete dictionary. Here, the overcomplete dictionary we use is the Ramanujan Basis A. 
The orthogonal matching pursuit algorithm is then applied onto the signal Y, and we have considered 1700 iterations.

The plot of the original sparse representation and the OMP representation is given below:

{{< figure src="plot1.jpg" title="Plot of original sparse representation (blue) and OMP representation (red)" numbered="true" lightbox="false" >}}

The image is then reconstructed by taking the inverse of the Ramanujan Basis, and multiplying it with the OMP sparse representation. 

$$R e c  =A^{-1} * x w s r$$

Where xwsr is the OMP representation, and Rec is the reconstructed image signal. This is then resized to obtain the final image, which has been compared with the original image below:

{{< figure src="orgvsfinal.jpg" title="Original and Final image" numbered="true" lightbox="false" width=500 >}}

### Results

We compare and evaluate the performance of the Compressive Sensing Algorithm by using PSNR, SSIM and MSE image evaluation metrics. 

* PSNR: Peak Signal to Noise Ratio 
* SSIM: Structural Similarity Index
* MSE: Mean Square Error

Ideally, high values of PSNR, SSIM(max=1) and MSE show favourable performance of the reconstruction algorithm.The results obtained for this approach are:
* PSNR = 23.1362
* SSIM = 0.6265
* MSE = 315.8316