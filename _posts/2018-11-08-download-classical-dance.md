---
toc: true
layout: post
title: Classify the Classical Dances of India - I
description: Download images of Different Classical Dances from Google
categories: [fast-ai, icrawler]
---
There are various classical dances that are available all throughout in India. These classical dances are culture specific. For example, Tamil Nadu follows a classical dance called Bharathanaatiyam where as its nearby state, Kerala has a classical dance of Kathakali.

Being not much knowledgable in any of the classical dances, I was triggered by the idea of Jeremy Howard who was suggesting that we might be able to solve deep learning problems even if we ourselves arent the domain experts. So, I decided to try myself out if I could build a model that can perform well in classifying the classical dance names.

# Steps

To build a classifier, we have to build a classifier on our own that can be run end to end, we need to follow the below steps.

- Download the dataset
- Do preprocessing of data, if any
- Create a model to detect the classical dance

I prefer installing the libraries usiing the notebook itself so that we can reuse it anytime as a package.

I use *icrawler* library to download data from the google and bing. Also *FastAI* library has this feature to check and delete the files which have the corrupted images.



```python
# !pip install icrawler
# !pip install fastai
```

Now that we have installed all the required libraries, we could start downloading the dataset.



```python
from icrawler.builtin import  GoogleImageCrawler
from fastai.vision import *
from pathlib import Path
from random import randint
import logging
```

We need to download images of few types of classical dance images for training the model.

```python
classes = ['Bharathanatyam','Kathakali','Kathak','jagoi dance']
```

After we downloaded the images, we need to check if the downloaded images have some corrupt images as well. These images will not open for some reason and we dont want to train our network with these images.

*Fast AI* comes with a method to verify these images using `verify_images` method

```python
for c in classes:
    print(c)
    download_directory = './data/dances/'+c
    search_filters_google = dict()
    google_crawler = GoogleImageCrawler(downloader_threads=4,storage={'root_dir': download_directory})
    google_crawler.crawl(keyword=c, filters=search_filters_google, max_num=1000)
    verify_images(download_directory, delete=True, max_workers=8)
```

Now that we have downloaded the images we first check if the folder is created correctly

```python
!ls data/dances
```

```
Bharathanatyam	jagoi dance  Kathak  Kathakali
```

Now we can randomly check if the files are created correctly

```python
!ls data/dances/'{classes[3]}'
```

```python
# !rm -r data/dances/*
```

```python
path = Path('data/dances/'+classes[randint(0,3)])
print(path.parts[-1])
pathiter = list(path.iterdir())

open_image(pathiter[randint(0,len(pathiter))])
```

Kathak

![png](..\img\posts\2018-11-08-classical-dance-i/output_16_1.png)

**Note**: This blog post is completely written in Jupyter notebook. Though, Some outputs that might be not suitable for blog reading are removed.



To create a model that learns from these images, look into the next part [here](https://nareshr8.github.io/2018-11-08-learn-identifying-classical-dances/)
