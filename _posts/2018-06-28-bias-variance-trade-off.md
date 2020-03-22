---
layout: post
title: Bias Variance Trade-off
subtitle: Tuning the bias and variance for model optimization
tags: [machine-learning]
published: false
---

Any machine learning model takes a set of inputs (X) and predicts one or more outputs (Y). Our goal as a data scientist might be to create a function that takes the inputs and formulate a way to get the output. We term this function as Model. Our goal is to create such a model which is good at predicting the outputs (Y).  As George Box said "All models are wrong but some are useful". 

We might start building a useful model that can predict the output to some extent. While building we may have lot of parameters to tune. Tuning model will have a side effect. If your model is so biased, tuning it against it might cause an increase in variance and vice versa. As Andew Ng describes, this is something like having a nozzle when tuned will decrease the bias thereby increasing the Variance (or vice-versa). In simple terms tuning for one, harmfully affects the other. This is bias-variance trade-off.

 
