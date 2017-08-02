---
layout: post
title: Neural Network Notes
date: 2017-08-01
tag: NN, ML
comments: true
---
# Neural Network Notes

## References:
Convolutional Neural Networks (CNN) have become an important tool for object recognition. Please check the cool content from <a> http://cs231n.github.io/</a> and <a>http://karpathy.github.io/neuralnets/</a>

## Introduction:
In most of our daily life, we need to do **"Analyze"** observation(**data/input**) in order to make certain decisions(**output**).  

People are tired of heurustic pattern, hence we have some nice mathmatical algorithm to ease the work:
- Simple Linear Function y = Ax
- Nonlinear Function y = f(x)

We always want to implement such mathmatical model into computer programs like the following:
```python
def model(data)
    # some matrix or function with parameters
    return decision
```

However, there is no obvious way to hard-code the parameters in the module. Finding matrix ***A*** and nonlinear function ***f(x)*** itself would be not applicable.

In order to find appropriate parameters, we can use machine learning to train the model.
```python
def train(trainData, pseudoDecisionSet)
    # Machine Learning
    return model
```
The module above eventually  ***"Memorize"*** all data and expected decisions. 

Eventually, we would love to test and use the trained model to predict and calculate our result:
```python
def predict(model, test_data)
    # Use the model we trained
    return test_decision
```


