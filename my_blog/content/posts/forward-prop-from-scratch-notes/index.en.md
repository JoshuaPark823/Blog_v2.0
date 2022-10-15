---
title: "Forward Propagation from Scratch (Notes)"
date: 2022-10-15
draft: true
tags: [
    'Machine Learning',
    'Neural Networks',
    'Python',
    'Notes'
]
---

# Introduction

Below are some rough notes on implementing forward propagation from scratch.
The material source is Andrew Ng's lecture on forward propagation in his course Advanced Learning Algorithms.

# Python Implementation

- Below are rough notes on a python implementation from scratch.

Dependencies

```
import numpy as np
import matplotlib.pyplot as plt
import tensorflow as tf
import logging
logging.getLogger("tensorflow").setLevel(logging.ERROR)
tf.autograph.set_verbosity(0)
```

Load the dataset and do some normalization

```
X,Y = load_data()

norm_l = tf.keras.layers.Normalization(axis=-1)
norm_l.adapt(X)  # learns mean, variance
Xn = norm_l(X)
```

Define the activation function

```
g = sigmoid
```

Define the function for a dense layer using the sigmoid activation function

```
def dense_scratch(a_in, W, b):
    units = W.shape[1]
    a_out = np.zeros(units)
    for j in range(units):               
        w = W[:,j]                                    
        z = np.dot(w, a_in) + b[j]         
        a_out[j] = g(z)               
    return(a_out)
```
Define a two layer neural network using the `dense_scratch()` subroutine

```
def sequential_scratch(x, W1, b1, W2, b2):
    a1 = dense_scratch(x,  W1, b1)
    a2 = dense_scratch(a1, W2, b2)
    return(a2)
```

Define some training values

```
W1_tmp = np.array( [[-8.93,  0.29, 12.9 ], [-0.1,  -7.32, 10.81]] )
b1_tmp = np.array( [-9.82, -9.28,  0.96] )
W2_tmp = np.array( [[-31.18], [-27.59], [-32.56]] )
b2_tmp = np.array( [15.41] )
```

Define the forward prop subroutine

```
def predict_scratch(X, W1, b1, W2, b2):
    m = X.shape[0]
    p = np.zeros((m,1))
    for i in range(m):
        p[i,0] = sequential_scratch(X[i], W1, b1, W2, b2)
    return(p)
```

Test the model against training exmaples

```
X_tst = np.array([
    [200,13.9],  # postive example
    [200,17]])   # negative example
X_tstn = norm_l(X_tst)
predictions = predict_scratch(X_tstn, W1_tmp, b1_tmp, W2_tmp, b2_tmp)
```

Apply a decision threshold

```
yhat = (predictions >= 0.5).astype(int)
print(f"decisions = \n{yhat}")
```

> Output: `decisions = 
[[1]
 [0]]
`