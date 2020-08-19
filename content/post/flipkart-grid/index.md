---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Flipkart Grid 2.0 Hackathon"
subtitle: ""
summary: "An article about our experience of taking part in our first hackathon"
authors: [Archana Swaminathan, Rushabh Musthayala]
tags: [Machine Learning, Computer Vision]
categories: [Experiences]
date: 2020-08-17T17:39:32+05:30
lastmod: 2020-08-17T17:39:32+05:30
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: true

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
url_code: "https://github.com/archana1998/Gradient-Ascent_FK-GRiD"
---

Flipkart recently concluded their 2 month long annual hackathon for students of Indian engineering colleges. This year’s edition saw over 20,000 participants and boasted of a prize pool of around Rs. 300,000 (-4000 USD). Our team (Gradient Ascent) made it to the 3rd round of the competition and I am writing about our experience in this article.

## Problem Statement 
A fashion retailer wants to source ongoing and upcoming fashion trends from major online fashion portals and online magazines in a consumable and actionable format, so that they are able to effectively and efficiently design an upcoming fashion product portfolio.

Deliverables:
  1. Identify products that are better performers (in a rank ordered fashion)
  2. Help the user view the products that are both trending and lagging
  3. Identify a logic for classifying products as per their trendiness

We were asked to complete the challenge for just the t-shirt product vertical, but to ensure that our solution would be scalable to other products as well.

## Initial Analysis

We started off by performing a literature review on current research in the field of fashion with respect to deep learning. We looked at previous attempts of learning attributes from fashion images, modelling trends as timeseries data, fashion image encodings, object detection, etc. 

After spending some time on our research, we split the problem into the following 
subproblems to tackle independently:

  1. Data Collection
  2. Object Detection
  3. Attribute/Feature learning
  4. Ranking 
  5. Grouping (trending/lagging)

## Data Collection

According to the problem statement, we had to extract data from e-Commerce sites and other fashion portals and magazines. We tried our best to include data from all those categories to ensure we had a balanced dataset for our classification and ranking later on. After scouring the web for some good resources, we finally settled on the following:

  1. Flipkart
  2. Amazon
  3. Pinterest (curated collections of fashion trends)
  4. Vogue India
  5. Myntra

We felt this combination of multipurpose e-Commerce sites, well established fashion magazines, social network sites and dedicated fashion shopping sites would ensure we had good representation from all sectors. We collected an average of around 600 images from each website, giving us a total of 3000 to work with.
Web scraping was done in Python using the Selenium framework. The scripts used to scrape data from any website were pretty similar and any new sites could be added with minor modifications, hence this step was easily scalable.
From e-commerce sites, we scraped the images, product names, ratings and the number of reviews to with ranking later on. From the other portals, we extracted just the images.

## Object Detection

One of the biggest problems we faced when extracting images from fashion magazines and social media sites is that they don’t limit themselves to just t-shirts. When they put out a catalogue/collection, it has everything ranging from skirts to sweaters to scarves. Furthermore, even in pictures where the shirt was the highlight, other features such as the model’s pose, skin colour and distance from the camera could confuse our model in the later stages of this project. Keeping all this in mind, we decided to use an object detection model to filter our data to ensure we had only pictures of t-shirts. Additionally, we cropped the images according to their bounding boxes to counter the other aforementioned problems.
This was done using a pretrained YOLOv3 model trained on the DeepFashion2 dataset, implemented using PyTorch.

## Attribute/Feature learning

This is where we faced our major setback. Our initial plan was to train a model to learn the attributes (neck type, sleeve length, patterns, etc) and to return them back for later use. We were then going to perform FP growth on our set of attributes of each image to obtain the frequent itemsets which would correspond to the most common combination of features and hence, trending/popular styles.

It didn't work out however, as we couldn’t find an appropriate dataset to work with such a task given our time constraints so we had to try out our backup plan.

Our plan involved getting numeric encodings for the fashion images in place of the attribute list and performing clustering on the encodings. The largest clusters would correspond to the most popular types of clothes, and similarly, the smallest clusters would represent the lagging ones, assuming our calculated encodings are a fair representation of the original image. Since we were working with just images (unlabeled) data, we had to devise an unsupervised approach for learning the image encodings. After considering various options, we decided to go ahead using an autoencoder based on a CNN architecture. We did this for 2 major reasons:
  1. Convolutional layers would help notice particular features of t-shirts such as the necktype length and patterns if any
  2. We can insight on how accurate our encodings to reconstruct the image

Here’s a summary of the model we used:

{{< figure src="1.png" title="Frequent Itemset Mining" numbered="true" lightbox="false" >}}

We then plotted some of the reconstructed images side by side with their original counterparts and got pretty good results considering the simplicity of the network and size of the dataset. The encodings were able to capture some important features of the clothes in question.

{{< figure src="2.png" title="Model Architecture" numbered="true" lightbox="false" >}}

## Ranking

As far as e-commerce sites go, there are 2 main criteria used to determine how “good” a product is – the number of reviews and the rating it has. What would you consider to be better? 10 reviews with a 5-star rating? Or 50 reviews with a 4.7-star rating? This was the major question we had to answer to be able to rank these products properly. We needed an effective way of combining these 2 into one reliable metric. After doing some research on this area and tying out different methods of combing them, we settled with an approach based on  a Bayesian view of the beta distribution, described beautifully in this video by <a href =" https://www.youtube.com/watch?v=8idr1WZ1A7Q&feature=emb_logo">3blue1brown</a>
We used this principle to come up with our own “Popularity Metric” which was calculated as follows:


{{< figure src="3.png" title="Reconstructed Images from encodings" numbered="true" lightbox="false" >}}


We now had a mechanism to compare and rank products effectively and a way to calculate accurate image encodings. We used both of these to train a model which predicts the Popularity Metric of a given clothing item given an input as the image encoding. We envisioned such a model to be extremely useful for designers that are looking for insight as to how their clothes might fair if they were put up for sale on e-commerce websites. Furthermore, the Popularity Metric could be calculated for all the images from magazines and portals like Vogue and Pinterest, so those products can be ranked and compared too!
The architecture, simplified pipeline and a screenshot of the program in action are shared below.


{{< figure src="4.png" title="Popularity Metric" numbered="true" lightbox="false" >}}
<center> (n = number of reviews, s = star rating ) </center>
{{< figure src="5.png" title="Popularity Metric Model" numbered="true" lightbox="false" >}}



## Grouping
Since the FP growth idea fell through the roof, we went with clustering as our method of choice for grouping products in such a way that we can obtain the trending and lagging items. To ensure our clustering was done well, we experimented on a variety of clustering algorithms and chose the one with the highest silhouette coefficient. The algorithms tested were –
  1. K means 
  2. Gaussian mixture model
  3. DBSCAN
  4. Mini batch k means
  5. Spectral clustering

Among those, K means had the highest silhouette efficient so we went ahead with that. We then split the data into clusters according to how many images were being considered for clustering (no. of clusters = no. of images/10). The products in the largest cluster could be inferred as the trending/popular products and those in the smallest clusters would be lagging products. We gave the user the option to spec which sources they wanted to consider for their clustering, giving them more flexibility with regards to analyzing what’s not and what’s not (what’s trending on Vogue might not be popular on Amazon). 


To conclude, we were able to come up with a way to rank products properly and to group them based on whether they are trending or lagging. We also ensured that our solution is scalable on 2 fronts:

  1. Getting more data can be done easily with minor modifications to the existing script
  2. We can expand to different product verticals by changing the object of interest in the object detection model
  
The link to the GitHub Repo is at the top of this page.
Hope you found this interesting, thanks for reading!