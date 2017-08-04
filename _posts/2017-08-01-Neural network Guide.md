---
layout: post
title: Neural Network Notes
date: 2017-08-02
tag: NN, ML
comments: true
---

## References:
Convolutional Neural Networks (CNN) have become an important tool for object recognition. Please check the cool content from <a> http://cs231n.github.io/</a> and <a>http://karpathy.github.io/neuralnets/</a>

## Introduction:
In most of our daily life, we need to do **"Analyze"** observation(**data/input**) in order to make certain decisions(**output**).  

People are tired of heuristic pattern, hence we have some nice mathematical algorithm to ease the work:
- Simple Linear Function y = Ax
- Nonlinear Function y = f(x)

We always want to implement such mathematical model into computer programs like the following:
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


Eventually, we want to use those observed data to get a proper model and even update our model. In linear classification, the parameter we tried to toggle with are pretty much the entries of ***A, B*** in ***y = Ax+B***. These types of linear model can be think of any kind of linear systems, e.g. could be circuits, networks and so on.

## Real Valued Circuits
One great way of understanding Neural Networks is to think them as real-valued circuit. We have *+*, *max*, *log()*, *exp()* and more computational functions. 

```javascript
var forwardMultiply = function (x,y){
    return x * y;
};
forwardMultiply(2,3); //returns 6
```

#### Core Question:
How should one tweak the input parameters slightly to increase/decrease the output?

- Random Local Search:
    use Math.random() to generate little tweak on *x* and *y*.
    If the result better than the current result, update it.
    -   Cost too much

- Analytic Gradient
In the example above, f(x,y) = x*y, we have
$frac{\partial f(x,y)}{\partial x} = y$.

Hence, we can transform the derivative calculation into
```javascript
var x_gradient = y;
var y_gradient = x;
var step_size = 0.01;

x + = step_size * x_gradient;
y + = step_size * y_gradient;

var out = forwardMultiply(x,y); // In this case, out is updated to 2.03*3.02
```

##### To Scale

The analytic gradient method works well and we can always use *Chain Rule* when the circuit scaled up. For example, if we computing f(x) = (x+y)z.

```javascript
var forwardMultiply = function(a, b){return a * b};
var forwardAdd = function(a, b){return a + b};

var fowardCircuit = function(x, y, z) {
    var q = forwardAdd(x, y);
    var f = forwardMultiply(q, z);
    return f;
};
var x = -2, y = 5, z = -4;
var f = forwardCircuit(x, y, z);
```









