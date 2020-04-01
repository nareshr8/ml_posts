---
toc: true
layout: post
title: Linear Regression
description: Linear Regression with one Variable
categories: [ml]
---

# Linear Regression with one Variable

Linear Regression is a class of supervised machine learning where we look for the answers that are real valued numbers and hence we call it **regression**.

## Predicting Housing Price

As a sample we take the task to predict the housing price of the houses in portland based on the size of the house.

We take this as a base sample data to understand the uni-variant linear regression problem. As we plot the price to size in a graph. It looks like the following.
![Housing Price Graph]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images/2e881f1d.png)

Our goal is to develop a machine learning algorithm that can train itself to find a function that can draw a line as below so that, when a new data, say house with size as 1250 feet², using the line, we could easily predict the price to be around $220K.
![Predicted Housing Price]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images/d3fc0ee1.png)

### Notations

To understand the notation, Lets take the pricing data as follow:


| Size in feet² (X)| Price ($) in 1000's(Y)|
| :------ |:--- |
|2104|460|
|1416|232|
|1534|315|
|852|178|
|...|...|

In the above table, each and every record is a training example that is fed to the system. The size of the house is the input to the machine learning system and it outputs the price as a real valued number. Hence, we denote the values in the following format.

> m - Number of training Examples 
>
> x - **input** variable / feature 
>
> y - **output** variable / **target** variable

To simplify things, (x,y) is denoted as one training example, whereas X(i),Y(i) represents ith training example.

So, in the above example, X(1)=2104 and Y(3)=315.

### Model

The System gets the training set, uses some algorithm to learn the hypothesis function. Using the hypothesis function (also called the model), we predict the price for any new given size of the house.
![Price Prediction]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images\7b1d86bb.png)

Hypothesis(h) is the function that maps the input X to the output Y, such that `Y=h(X)`. 

The hypothesis can be represented as the following formula, for a uni-variant model
> h<sub>Θ</sub>(X) = Θ<sub>0</sub>+Θ<sub>1</sub>(X)
<br>where Θ<sub>0</sub> and Θ<sub>1</sub> are parameters of the model

So, For **Θ<sub>0</sub>=1.5** and **Θ<sub>1</sub>=0** the hypothesis looks like:
![Hypothesis Function]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images\084a51aa.png)

And For **Θ<sub>0</sub>=0** and **Θ<sub>1</sub>=0.5** the hypothesis looks like:
![Hypothesis Function]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images\717415b9.png)

So, For **Θ<sub>1</sub>=0** and **Θ<sub>1</sub>=0.5** the hypothesis looks like:
![Hypothesis Function]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images\636e7675.png)


### Cost Function

Now, lets bring the graph and training examples together.
![Hypothesis Function with training examples]({{ site.baseurl }}/images/posts/2018-10-03-linear-regression_images\56c08adc.png)

Here, the hypothesis function is the line for a given  Θ<sub>0</sub> and  Θ<sub>1</sub>. And the X denotes the training data for say, the housing price.

We need the hypothesis function to be very close to value of Y, so that we can use this hypothesis function to calculate the output for any new input data. In other words, we need to choose Θ<sub>0</sub>,Θ<sub>1</sub> so that h(X) is very close to Y. 

To rephrase it to a mathematical notation, our goal is to minimize the value of the difference between hypothesis and output value of the training data.

So, we are trying to minimise the squared difference between the hypothesis and actual value and averange across all the **m** training examples. We then half the difference to make the number smaller for caluclation.

The overall goal is
$$
min_{ \theta _{0},\theta _{1}}  \frac{1}{2m}  \sum_{i=1}^{m}(h(x^{(i)})-y^{(i)})^2
$$

Here we are trying to find the squared difference because it is the most commonly used method that works reasonably well than most other cost functions in a wide variety of applications.

Now, we specify the cost function as 
>$$
>J(\theta _{0},\theta _{1}) = \frac{1}{2m} \sum_{i=1}^{m}(h(X^{(i)})-Y^{(i)})^2
>$$
>

 And our overall goal is to minimize this cost function:
>$$
>min_{\theta _{0},\theta _{1}}  J(\theta_0,\theta _{1})
>$$
>

#### Intuition

To grow a better intuition about what cost function is and what the minimization of cost function does, we consider $$Θ_0$$to be 0.

So, our overall goal is 

> $$
> min_{\theta_1} J({\theta_1}) \quad where \quad J_{\theta _{1}} = \frac{1}{2m}\sum_{i=1}^{m} (h_{\theta _{1}}(X^{(i)})-Y^{(i)})^2
> $$
>

Now that we have a goal to achieve. We will plot two graphs. One of the predicted output `h(X)` which we will use to plot the second graph to plot $J_{\theta_{1}}$ so that we can understand what exactly $min_{\theta_{1}}$ means.

### Vertical Bar
 <div id="vis"></div>

function parse(spec) {
  vg.parse.spec(spec, function(chart) { chart({el:"#vis"}).update(); });
}
parse({"name":"Vega Visualization","height":450,"padding":"auto","marks":[{"properties":{"enter":{"x":{"field":"x","scale":"x"},"y2":{"field":"y2","scale":"y"},"width":{"offset":-1,"scale":"x","band":true},"fill":{"field":"group","scale":"group"},"y":{"field":"y","scale":"y"}}},"from":{"data":"table"},"type":"rect"}],"axes":[{"properties":{"title":{"fontSize":{"value":14}}},"title":"x","type":"x","scale":"x"},{"titleOffset":40,"properties":{"title":{"fontSize":{"value":14}}},"title":"y","type":"y","scale":"y"}],"data":[{"name":"table","values":[{"x":1,"y2":0,"group":1,"y":1},{"x":2,"y2":0,"group":1,"y":2},{"x":3,"y2":0,"group":1,"y":3},{"x":4,"y2":0,"group":1,"y":2},{"x":5,"y2":0,"group":1,"y":1}]}],"scales":[{"name":"x","range":"width","domain":{"data":"table","field":"x"},"type":"ordinal"},{"name":"y","range":"height","domain":{"data":"table","field":"y"},"type":"linear"},{"name":"group","range":["rgb(166,206,227)","rgb( 31,120,180)","rgb(178,223,138)","rgb( 51,160, 44)","rgb(251,154,153)","rgb(227, 26, 28)","rgb(253,191,111)","rgb(255,127,  0)","rgb(202,178,214)","rgb(106, 61,154)","rgb(255,255,153)","rgb(177, 89, 40)"],"domain":{"data":"table","field":"group"},"type":"ordinal"}],"width":450});
</script>

> Disclaimer: Most part of the contents in this blog are from the [Machine Learning](https://www.coursera.org/learn/machine-learning) course by Andrew Ng.
