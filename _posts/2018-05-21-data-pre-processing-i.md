---
toc: true
layout: post
title: Preprocessing Structured Data For Machine Learning - I
description: Preprocessing data in Python 
gh-repo: nareshr8/ML-Notebooks
gh-badge: [star, fork, follow]
categories: [preprocessing,machine-learning]
---

Preprocessing the data is the first part of any machine learning project that we take up. This blog post, being my first ever, will start to discuss about preprocessing of data in python. I used Jupyter Notebook/lab as the IDE of preference along side Python 3.

# Why Data Preprocessing?

Data Preprocessing ensures that the data is available in the right format for machine learning to be performed. 

The golden rule for any machine learning project is more the data, the better. So, We might collect data from various sources. All data may not be good straight away for starting machine learning. We may run into one or many of these problems, most of the time:

- **Missing Data** - Some columns might be missing in some rows. 
- **Incorrect Data** - Some data might have been wrong due to manual entry or inconsistent data source
- **Feature Scaling** - Algorithms such as KNN, SVM prefer uniform distribution among the dataset as it uses distance or similarities between datasets.

Now that the need of preprocessing is felt, we can start to preprocess the data. Here, the theme is to get started building a notebook with functions that are commonly used in preprocessing structured data that can be used for any structured data.

# Prerequisites

We are using the Following libraries for making our process easier.

- Pandas

  To install run this on your python console :

  ```shell
  !pip install pandas
  ```

  ​

- Numpy

  To install run this on your python console :

  ```shell
  !pip install numpy
  ```


A simple google might solve the problem if you have any trouble installing these packages.

# Preprocessing Steps

First we have to import our necessary packages

```python
import numpy as np
import pandas as pd
```

 

Now collect the data into Data Frame. We can collect data from different sources into a Data Frame. Since we are creating the just utilities that take in data frame as input, we can skip this step.

#### Getting list of Missing Values

We may have to deal with the fact that there will be missing values in one or more columns in our dataset. First we may have to take a look on how many columns have missing values. So we can have a helper method do that for us.

```python
def get_missing_valued_columns_list(dataset):
    return dataset.columns[dataset,isnull().any()]
```

This gives the list of columns which has missing values.

Now that we see the list of columns which has missing values, we might be interested in knowing how many values among those are missing. If there are columns that misses more than, say 80% of data, we might chose to ignore that column.

#### Getting List of Columns with missing count

Now we look to get the data that shows the list of columns with the count of data that is missing in those column

```python
def get_missing_valued_column_details(dataset):
    sum_of_missing_values = dataset.isnull().sum(axis=0)
    return sum_of_missing_values[sum_of_missing_values > 0]
```

Here, we first summed up the count of data that has values as null (along the vertical axis), then we filtered out non empty columns.

#### Get labels which doesn't have enough data

If our label doesn't have enough data, we cannot make useful predictions out of the data. Thus, can remove columns that doesn't have enough data in them.

```python
def get_low_variance_columns(dataset):
	from sklearn.feature_selection import VarianceThreshold
    columns = dataset.iloc[:,:-1].columns
    selector = VarianceThreshold(.8 * (1-.8))
    selector.fit(dataset.iloc[:,:-1])
    labels = [columns[x] for x in selector.get_support(indices=True) if x]
    return labels
```

Here we are selecting all columns except the last one, as last column is usually the output or prediction column. Then we use VarianceThreshold function thats part of scikit learn library to specify the threshold (.8) to retrieve the list of columns. 


I am planning to add other commonly used functions in my next blog post. Comment on your views.