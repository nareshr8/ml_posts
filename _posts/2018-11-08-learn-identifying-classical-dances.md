---
toc: true
layout: post
title: Classify the Classical Dances of India - II
description: Learn Identifying Classical Dances
categories: [fast-ai, icrawler]
---
Now that we have downloaded the images [here](https://nareshr8.github.io/2018-11-08-download-classical-dance/). Our next step is to train the model to learn identifying classical dance images.

## Look into the images

First we will import the necessary package


```python
import numpy as np
from fastai.vision import *
from fastai import *
from pathlib import Path
```

Now that we imported all the libraries we want, we can take the next step. Load the images into various bunches such as training and testing sets.

We usually seed the random so that we can monitor the performance. If we see a improvement in accuracy, it must mean that our tuning worked and not that we were lucky to get the best data in the random shuffle.


```python
np.random.seed(8)
PATH= Path('./data/dances')
data = ImageDataBunch.from_folder(PATH, valid_pct=0.2, train=".", size=224,ds_tfms=get_transforms(),num_workers=4).normalize(imagenet_stats)
```

In the above code 
- We have made sure the same set of images are used evertime we randomly divide the dataset into training and test set.
- We provide the path in which all our images are available
- We create a Data Bunch which takes the parameter like
    - `PATH` path of the images
    - `valid_pct` percentage of data that is available in validation set
    - `size` size of the images (generally 224\*224 is prefered)
    - `ds_tfms` the required transformations that are needed. Like trimming, zooming and so on
    - `num_workers` the total number of worker threads that are needed to do the computations like transformation
- We normalise the data so that the images are normalised the same way the pre trained model like *resnet34* are normalised

Now we can do some basic analysis to see if we got the data right


```python
data.classes
```




    ['Bharathanatyam', 'Kathak', 'Kathakali', 'jagoi dance']




```python
data.show_batch(rows=3, figsize=(7,8))
```


![png](..\img\posts\2018-11-08-learn-identifying-classical-dances\output_10_0.png)



```python
data.classes, data.c, len(data.train_ds), len(data.valid_ds)
```




    (['Bharathanatyam', 'Kathak', 'Kathakali', 'jagoi dance'], 4, 1957, 496)



Now after some analysis we find that these images like ok. So, leets start by looking into train the machine

## Training the model

As we are all set for training the machine, we can continue to train the machine learn the type of dance from image


```python
learn = create_cnn(data=data,arch=models.resnet34, metrics=error_rate)
```

Note that the method `error_rate` is available in the package *fastai*. So, getting the import is essential apart from *fastai.vision*

We'll now train the model


```python
learn.fit_one_cycle(4)
```

    Total time: 02:33
    epoch  train_loss  valid_loss  error_rate
    1      1.074674    0.658487    0.231855    (00:41)
    2      0.814936    0.592159    0.211694    (00:39)
    3      0.668678    0.592504    0.199597    (00:36)
    4      0.570397    0.591368    0.189516    (00:36)



On training the last layer, we get an error of *18%* which is good start. We save the model as a checkpoint now.


```python
learn.save('stage-1')
```

Now lets unfreeze all layers and try to make the entire neural network learn


```python
learn.unfreeze()
```

Now, lets try to identify the learning rate


```python
learn.lr_find()
```

    LR Finder complete, type {learner_name}.recorder.plot() to see the graph.



```python
learn.recorder.plot(2)
```


![png](..\img\posts\2018-11-08-learn-identifying-classical-dances\output_25_0.png)


Now, we see that somewhere around $$3e^-6$$ to be a good spot


```python
learn.fit_one_cycle(4, max_lr=slice(3e-7,1e-6))
```

    Total time: 02:29
    epoch  train_loss  valid_loss  error_rate
    1      0.453784    0.588463    0.191532    (00:37)
    2      0.463598    0.588832    0.189516    (00:37)
    3      0.472005    0.588780    0.191532    (00:38)
    4      0.458501    0.586727    0.189516    (00:36)




```python
learn.save('stage-2')
```

## Understanding the model

After we save the model, we will analyse the model so that we can get enough information out of it


```python
learn.load('stage-2')
```


```python
interp = ClassificationInterpretation.from_learner(learn)
```

**Confusion Matrix** is a matrix which tabulates the Actual Vs Predicted output. If both are same, then out machine did great job in identifying the image. Lets see how our model preformed


```python
interp.plot_confusion_matrix()
```


![png](..\img\posts\2018-11-08-learn-identifying-classical-dances\output_34_0.png)


As the picture says, the machine confuses a lot regarding *Bharathanatyam*. Well being from the region where it is famus, I might have some knowledge, lets see if thats helpful.

## Cleanup

There can be cases where some images are messy. Like, Google can give a search result which has a dancer who is famous in, say Bharathanatyam performing Kathak. These images can confuse the model in learning. Lets look if thats the case 

*FastAI* comes with this very cool widget which is very useful in these cases


```python
from fastai.widgets import *

losses,idxs = interp.top_losses()
top_loss_paths = data.valid_ds.x[idxs]
```

We got the list of losses and the top losses that are available. We canuse the widget to look for messy images


```python
fd = FileDeleter(file_paths=top_loss_paths)
```


    HBox(children=(VBox(children=(Image(value=b'\xff\xd8\xff\xe0\x00\x10JFIF\x00\x01\x01\x01\x00H\x00H\x00\x00\xff…



    Button(button_style='primary', description='Confirm', style=ButtonStyle())


Some common pitfalls that we can see is:
- Many examples had images of probably famous dances that were taken during some interview or award ceremony
- Agenda of the dance festival
- The ornaments and dresses thats associated with that dance
- The image of artist while they do the make over or dressing while preparing for the dance.\

Now, lets rerun the test. Find the learning rate, train the model...

One thing to note. Since we deleted the images which had these pitfalls, we need to recreate the model with the available data, load the last best model and run these tests.


```python
learn.lr_find()
```

    LR Finder complete, type {learner_name}.recorder.plot() to see the graph.



```python
learn.recorder.plot()
```


![png](..\img\posts\2018-11-08-learn-identifying-classical-dances\output_44_0.png)



```python
learn.fit_one_cycle(4, max_lr=slice(8e-8,1e-6))
```

    Total time: 02:31
    epoch  train_loss  valid_loss  error_rate
    1      0.508052    0.322376    0.106029    (00:38)
    2      0.501633    0.319395    0.106029    (00:36)
    3      0.498382    0.320848    0.106029    (00:36)
    4      0.497239    0.319271    0.112266    (00:38)



Now, our new model looks better. Has around *11.2%* errors, which is like 88.8% accurate. That looks pleasing. Can we do better? Will update the blog if I do so.
