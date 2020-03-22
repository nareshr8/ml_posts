---
toc: true
layout: post
title: Object Localization
description: Identify where the stamp is available in the image
gh-repo: nareshr8/Image-Localisation
gh-badge: [star, fork, follow]
categories: [object-localisation,CNN,ResNet,fast-ai,fastai]
--- 

Today, we work for a particular problem statement, image localization. Image Localization basically means that we will look into a given image and not just say whether a stamp is available in the picture or not but also where it is, if available.

The problem statement is easy for a kid to perform, but was impossible few years back for even super computers to accurately say like a kid. Thanks to the advancement in computational power and deep learning literature, we are now able to make computers do this task.

We are using a python library "Fast AI" to perform this task. If you are little unfamiliar with Fast AI, checkout fast.ai. It’s the easiest way to get your hands on machine learning and do some exciting tasks straight up.

# Problem Statement
Your company sends the manufactured products out of the garage only when they have been tested. The tested products have a seal like this:![Stamp Image](../img/stamp-image.png) 

You are a data scientist or a machine learning engineer who is trying to use computers to check if the product is tested or not instead of humans, as it speeds up the disposal of the products.
# Why Data Augmentation
Most of the time the company doesnt have enough images to perform machine learning tasks on. This is also a typical case in your company. Your company can only give you the image of the stamp. You try to collect the data yourself. However, You need a camera to take pictures on the product's cover art so that you can collect thousands of images. Your company asks for a proof that your product will work before it can install the camera on the product disposal area. This lands you in a place where you have to get the product working before the main component of the product (data) is available. So, we have to augment the data.
# Data Augmentation
We plan to augment whatever data is needed. We crawl through the web and pull images of various products and superimpose this stamp over them to create data that we are looking for. We thereby have a set of images that is available with/without stamp and the location if its available.
## Image Crawling
For image crawling in python we have a library 'icrawler', which I have used and seem to pull images from different data sources like Google, Bing, Baidu. We can use the crawler to pull the images from internet. The working crawler notebook is available [here](https://github.com/nareshr8/Image-Localisation/blob/master/crawler.ipynb).

### Why crawl thousands of images?
If we have only 10 images in training set as background and superimpose the stamp in various locations, there is a possibility of overfitting, meaning that the machine will try to remove the 10 possible backgrounds and check if any of the remaining pixel is having the value, instead of looking at the stamp. Which will not be the ideal scenario for the actual images. So, even if its augmentation, the more data the better.
## Image Super-Imposition
I have used PIL library to super impose the images. The image preprocessing is done in [this](https://github.com/nareshr8/Image-Localisation/blob/master/Image%20PreProcessing.ipynb) notebook.
# Training the Model
For training the model, we train the model using the notebook that is available [here](https://github.com/nareshr8/Image-Localisation/blob/master/Localisation.ipynb).
# Other variants
Here we used only one stamp and we are checking if that particular stamp is available. Instead, we can generate data with all the variety of labels with all variants of stamps as well and pretty much follow the same procedure. We would be able to train the model to check for any of the stamps is available and the location.
	The other alternative is Person Tagging. Similar to stamps, we can give a set of images with people faces and ask the machine to tag the person's face on the image.
# Improvisation
As part of improvising, we can
- tag multiple items on the same image.
- identify each class of the tagged image.
- Use this process as part of a end to end solution. 
		<br><i>For example, if we want to know the price of a product, we naturally find the location where the price is listed. After localizing the place where the price is listed, we try to read the price. We can do the same with machines to understand the price of the product. </i>
- Use to develop Optical Character Recognition. We tag each character and a classifier that classifies between A-Z and Numbers. The characters with lesser space between them forms a word.


I may try some of these myself and post if something really cool works out.

Post your comments and let know your views on this.

