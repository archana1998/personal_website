---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Image Encryption and Decryption using Artificial Neural Networks"
summary: "Developed a novel algorithm for image encryption using Artificial Neural Networks."
authors: [Archana Swaminathan]
tags: [Cryptography, Machine Learning]
categories: [Undergraduate Projects]
date: 2020-07-30T12:05:59+05:30

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: true

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: "https://github.com/archana1998/image-encryption"
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
# Introduction

This project is inspired from the schematic described in the paper by I. A. Ismail
and Galal H. Galal-Edeen. A multilayer perceptron network is used for both the
encryption and decryption of images. The keys used for decryption are the fixed
bias vectors, which remain constant throughout training. Multiplicative neural
networks are used to help generate this constant vector, which is derived from a
vector specified by the sender. The images are sent into the neural network and
the output of the hidden layer gives the cipher. The cipher on being passed into
the output layer gives the decrypted image. The weights are trained and updated
using the backpropagation algorithm for learning, while the bias vector remains
constant.

To get the constant bias vectors, the sender of the image specifies a numeric
vector of the same size as the layer it is a bias of. The vector is broken down
into subvectors and subsequent permutations of this vector are fed into a
multiplicative neuron. The output of the multiplicative neural network will be
added to the initial bias vector specified by the sender of the images. Since the
bias vector is now a constant that is entirely dependent on the way the initial
6bias vector is arranged, it provides an additional level of security over the
existing paradigm that employs a sender specified bias vector without any
modifications done to it.
All experiments have been done on, and results have been obtained from
MATLAB R2018b.



# Multiplicative Neural Network

A general structure for the multiplicative neuron is given below:


{{< figure src="1.jpg" title="Structure of multiplicative neuron" numbered="true" lightbox="false" >}}

The input vector is (x 1 , x 2 ,.....,x n ) which is a permutation of the initial specified bias vector. The weights vector is (w 1 , w 2, ....., wn) and the bias vector (for this multiplicative neural network) is (b 1 , b 2, ....., bn). Ω is a multiplicative operator and has the formula

$$\Omega=\prod_{i=1}^{n}\left(w_{i} x_{i}+b_{i}\right)$$

The output of the neuron is then processed using ƒ(u) which is the logsig function $y=\frac{1}{1+e^{-u}}$.


## Training Algorithm

The standard backpropagation algorithm has been modified for the training of
the multiplicative neural network, which is used in optimizing the weights and
biases. It is based on the popular steepest gradient descent approach. The error
function E is defined as

$$E=\frac{1}{2 N} \sum_{p=1}^{N}\left(y_{p}-y_{p}^{d}\right)^{2}$$

where $\ y_{p}^{d}$ is the desired output and y<sub>p</sub> is the actual output for the p th input that is fed into the multiplicative neural network. The weights and biases of the model are updated using the following rules:


$$w_{i}^{\text {new}}=w_{i}^{\text {old}}+\Delta w_{i}$$


$$b_{i}^{\text {new}}=b_{i}^{\text {old}}+\Delta b_{i}$$

where

$$\Delta w_{i}=-\eta \frac{d \boldsymbol{E}}{d w_{i}}=-\eta \frac{1}{N} \sum_{p=1}^{N}\left(\left(y_{p}-y_{p}^{d}\right) y_{p}\left(1-y_{p}\right) \frac{u}{w_{i} x_{i}+b_{i}} x_{i}\right)$$

$$\Delta b_{i}=-\eta \frac{d \boldsymbol{E}}{d b_{i}}=-\eta \frac{1}{N} \sum_{p=1}^{N}\left(\left(y_{p}-y_{p}^{d}\right) y_{p}\left(1-y_{p}\right) \frac{u}{w_{i} x_{i}+b_{i}}\right)$$

$\eta$ is the learning rate parameter. The main purpose of this parameter is to control the convergent speed as desired.

## Procedure

Two multiplicative neural network structures are used to generate the bias
vector for the hidden layer and output layer of the multilayer perceptron
network respectively. It is necessary to have a separate model for each vector as
the layers are of different dimensions (different number of neurons) and thus,
the bias vectors will be of different dimensions for both the hidden layer and the
output layer. The number of elements in the bias vector will be the size of the
input into the multiplicative neural network model, hence we require two
different models.

The sender of the images first specifies a vector containing the same number of
elements as the number of neurons of the (hidden/output) layer of the MLP.
This is now broken down into subvectors (if the number of elements is 16, it can
be broken into 4 subvectors of 4 elements each, etc.). This breaking down is
essential as it becomes computationally difficult to calculate the permutations of
a combination of numbers greater than 10. Once the subvectors are obtained, the
individual permutations of each of the subvectors is stored into a matrix, which
are then concatenated to form a bigger matrix of size p*q where p is the number
of subvectors, and q is the dimension of the initially specified bias vector. A
target vector (dummy vector, but must remain constant and not be generated
randomly) is also specified by the sender.

This matrix is now fed into as input to the multiplicative neural network, where
each row depicts an input sample. The network is trained using the algorithm specified, and the output is stored. This output is now added to the initial vector
specified by the sender for the MLP, and the result thus becomes the new bias
vector, which remains a constant throughout the experiment.

This procedure is repeated for defining and training the second multiplicative
neural network, which generates the second bias vector for the MLP. Both these
vectors are essential for proper image encryption and decryption.

# MLP used for image encryption and decryption

The network has a structure of one input layer, one hidden layer, and one output
layer. Adding further hidden layers can help in achieving image compression as
dimensionality of the image is being reduced. The output of the hidden layer gives the cipher and the output of the output layer gives the decipher of the image. There are N elements in the input layer that are fed to the next (hidden layer), which consists of M neurons. The output layer has the same number of neurons as the input layer.

The MLP structure used in this project is given below.


{{< figure src="2.jpg" title="Structure of MLP" numbered="true" lightbox="false" >}}

The network configuration is of NxMxN neurons which represent the input layer, hidden layer and output layer respectively. The sigmoid function (logsig) is used to generate the output of the hidden layer, which is defined as

$$\text { Logsig function: } Z=\frac{1}{1+e^{\left[-\left(\left(\sum_{i=1}^{n} w_{1 i} x_{i}\right)+b_{1 i}\right)\right]}}$$

where w<sub>1i</sub> denotes the weight vector for the hidden layer, and b<sub>1i</sub> denotes the bias vector for the hidden layer.

The output of the output layer is calculated using a linear function (purelin in
MATLAB), which is defined as

$$\text { Purelin function: } Y=m\left[\left(\sum_{i=1}^{n} w_{2 i} z_{i}\right)+b_{2 i}\right]+c$$

where w<sub>2i</sub> denotes the weight vector for the output layer, and b<sub>2i</sub> denotes the bias vector for the output layer.

## Training Algorithm

The error of the output of the network in each step ‘n’ while training, (the
difference between the desired value and the actual value) is calculated by the
following formula.

$$\Delta(n)=\left[\sum_{i=1}^{N}\left(x_{i}-y_{i}\right)^{2}\right]^{1 / 2}$$

The weights are calculated using the following rules:
1. For the hidden-output layer:

    a. The error signal for the q th neuron in the output layer is

    $$\delta_{q}=m\left(x_{q}-y_{q}\right)$$

    b. The updated weight w<sub>2(p, q)</sub> is calculated as:

    $$\begin{array}{c}w_{2(p, q)}(n+1)=w_{2(p, q)}(n)+\Delta w_{2(p, q)}(n+1)\end{array}$$
    
    $$\begin{array}{c}\Delta w_{2(p, q)}(n+1)=\eta \delta_{q} z_{p}+\alpha\left[\Delta w_{2(p, q)}(n)\right]\end{array}$$

2. For the input-hidden layer:

    a. The error signal for p<sup>th</sup> hidden neuron is calculated using:

    $$\delta_{p}=z_{p}\left(1-z_{p}\right)\left[\sum_{k=1}^{N} \delta_{k} w_{1(p,k)}\right]$$

    b. The weight vector w<sub>1(i,p)</sub>(n) is calculated similarly as the above
      formula for the adjustment of weights, with z<sub>p</sub> being replaced with
      x<sub>i</sub> and $\delta_{q}$ with $\delta_{p}$ .

3. After one epoch, let $\Delta(n)$ and $\Delta(n+1)$ be the previous and current errors of the outputs of the neural network respectively. The rule that is followed
whether to decide if the weights are being updated or not, is:

    a. If $\Delta(n+1)>1.04[\Delta(n)]$,
    The new weights, output, keys (always constant), error are
    unchanged, and $\alpha$ is changed to $0.7 \alpha$
    
    b. If $\Delta(n+1)<=1.04[\Delta(n)]$,
    All the variables except the keys are updated to their new values,
    and $\alpha$ is modified to $1.05 \alpha$


These steps are carried out and repeated in each epoch, until the maximum
number of epochs has been reached, or the error becomes less than a value that
is predefined.

## Procedure

The keys obtained from the multiplicative neural network are first normalized to
lie within the range of (0,1). The normalization is simply done by dividing the
elements of the bias vector by the maximum value of the elements of the vector.

The images that are fed into the neural network must all be of the same dimension, irrespective of them being training images or test images. For this project, images of various dimensions (256 x 256, 512 x 512 etc) have been scaled down to a dimension of 50 x 50. The images of this specified dimension are now segmented into sub images, of the number L (for the purpose of this project, L=100). The size of each sub image is x times x = N pixels, which makes N = 25. The segmentation is done using a custom defined segmentation function, that also converts each sub image into a 1-dimensional vector, and creates a matrix of dimension N x L (25 x 100) which is then fed in as input to the neural network.

Once the network is trained with the training set, it is ready to encrypt and decrypt images. The test images are segmented using the same segmentation function which was used to segment the training images, and are fed into the trained neural network as input.

The output of the hidden layer is computed using the output function (logsig),
which was previously defined. Since the size of the input image is NL, the output of the hidden layer is a matrix of size ML. (For this project, N = 25 and M = 16). The encrypted image (cipher) is then obtained after the output matrix is transformed into a 2-D matrix, for which the segmented images must be properly arranged back.

The decrypted image is obtained when the output of the hidden layer is fed into
the output layer. In short, it is the final output of the neural network and can
directly be computed by feeding the input test image into the neural network.


## Experiments and results

The project was done on MATLAB version R2018b on a computer with Intel
Core i5 6 th generation processor.

The neural network was trained using 38 test images, out of which 14 were colour images, all downloaded from the USC Vision Database. The 14 colour images and any subsequent images henceforth used for testing were all converted into single channel images. The test images were segmented and passed into the neural network, which took approximately 40 seconds to train. The initial bias vectors were specified by the programmer and then was input into the two multiplicative neural networks to generate new elements, which were then added with the previous bias vector. Only after this, the bias vector for the MLP was fixed with this value and the MLP was made to train.

Encryption and decryption of a test image of the Earth was extremely fast, with
the NPCR and UACI tests giving scores of 99.9665% and 0.34916 respectively. The PSNR ratios for the original image with the decrypted image and the cipher were 39.4156 and 39.3973 respectively.