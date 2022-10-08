---
title: "The Sigmoid, Logistic Loss, and Regularized Logistic Regression (Citrus Classification)"
date: 2022-10-07
draft: false
tags: [
    'Machine Learning',
    'Supervised Learning',
    'Logistic Regression',
    'Classification',
    'Python'
]
resources:
- name: "featured-image-preview"
  src: "citrus_diagram_1.png"
---

# Introduction

In this post I'll be covering some of the intuition behind logistic regression, the sigmoid, and the logistic loss function through a binary classification exercise using a random citrus (orange/grapefruit) dataset I found on Kaggle.

# Why not MSE for Logistic Regression?

Logistic regression is a statistical method that predicts categorical dependent variables given a set of independent variables. Since logistic regression is considered a generalized linear model because the outcome is reliant on the sum of the inputs and parameters, why can't we use the MSE cost function as we do with linear regression? Referring to an earlier post talking about the intuition behind the MSE, the reason it's so useful for linear regression is that the function is convex - meaning that performing gradient descent on the the cost, `J`, would eventually lead to a minimum.

However, when using the MSE for logistic regression, the problem **is** convexity. The MSE isn't guaranteed to be convex since it's a linear combination of scalar inputs and the non-convex logistic function. Since gradient descent kind of "wiggles" its way down to the minimum, what happens when the cost function is not convex? Unfortunately, this leads to gradient descent getting "stuck" in one of the many local minima and thus functions incorrectly.
This leads us to the use of the cross-entropy loss function (aka log loss) that I'll talk about below.

# The Sigmoid Function & Logistic Loss

For logistic regression, the model can be represented as follows:

$$ f_{w,b}(x) = \sigma(\hat{w}\cdot{\hat{x}} + b)$$

Where $\hat{w}\cdot{\hat{x}}$ is the dot product of the vectors `w` and `x` and `b` is again a scalar model parameter. The output is then passed through the sigmoid function that maps the values between 0 and 1. It's definition is shown below. For large positive values of x, the function maps the output to 1 and maps to 0 for large negative values of x. Evaluating the sigmoid function at 0 should give exactly 0.5. We call this value the **decision boundary** since it acts as the threshold of which class is applied to the value `x`.

$$\sigma(z) = \frac{1}{1+e^{-z}}$$

Probabilistically, it represents the probability $P(y=1|x)$ which defines the probability that the class is 1 given `x` observation.

The cost function for logistic regression differs slightly as well. First well begin by defining a loss function $loss(f_{\mathbf{w},b}(\mathbf{x}^{(i)}), y^{(i)})$ that defines the cost regarding a single data point.

$$
loss(f_{\mathbf{w},b}(\mathbf{x}^{(i)}), y^{(i)}) = (-y^{(i)} \log\left(f_{\mathbf{w},b}\left( \mathbf{x}^{(i)} \right) \right) - \left( 1 - y^{(i)}\right) \log \left( 1 - f_{\mathbf{w},b}\left( \mathbf{x}^{(i)} \right) \right)
$$

Where $f_{\mathbf{w},b}(\mathbf{x}^{(i)})$ represents the model prediction shown above and $y^{(i)}$ represents the target class. To keep the scope of the post contained I won't be covering the derivation, but the log loss function is guaranteed to be convex (and thus appropriate for gradient descent).

Now summing the log loss across all training examples and computing the average results in the cost function for logistic regression.

$$ J(\mathbf{w},b) = \frac{1}{m}\sum_{i=0}^{m-1} \left[ loss(f_{\mathbf{w},b}(\mathbf{x}^{(i)}), y^{(i)}) \right]$$

# Overfitting & Regularization

I could move on to gradient descent and have some success with logistic regression at this point. However, a common problem with classification models is that of **overfitting**. Overfitting occurs when the model fits extremely tightly to the training data and is unable to generalize well when applied to newer data points. In order to combat overfitting, I'll be introducing another term, the regularization term, that allows the model to selectively "shrink" certain features to mitigate their impact. 

$$
\frac{\lambda}{2m}  \sum_{j=0}^{n-1} w_j^2
$$

Where lambda represents the parameter that controls the amount of regularization applied to the model. If lambda is set to 0, there will be no regularization wheras if it were a large value it would have a proportionally large influence on shrinking the model coefficients towards zero. Applying the regularization term to our cost function we have the new cost function below.

$$ J(\mathbf{w},b) = \frac{1}{m}\sum_{i=0}^{m-1} \left[ loss(f_{\mathbf{w},b}(\mathbf{x}^{(i)}), y^{(i)}) \right] + \frac{\lambda}{2m}  \sum_{j=0}^{n-1} w_j^2$$

The above cost function for regularized logistic regression uses **L2 regularization**, or the L2 norm, where we force the coefficients to be extremely small but not necessarily 0. Notice that L2 regularization penalizes the model by adding the sum of squares of all the feature weights.

> Differences between L1 and L2 regularization will be covered in a future post

# Gradient Descent for Regularized Logistic Regression

Note that I'm not applying any regularization to the `b` parameter since it's a scalar value. The partial derivative of `J` with respect to `b` is shown below.

$$\frac{\partial J(\mathbf{w},b)}{\partial b} = \frac{1}{m}  \sum_{i=0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})  $$

And the partial derivative of `J` with respect to `w` is below. The derivative is actually very similar to non-regularized logistic regression with the exception of the L2 regularization term that's been manipulated by some derivation laws.

$$
\frac{\partial J(\mathbf{w},b)}{\partial w_j} = \frac{1}{m}  \sum_{i=0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)}) x_j^{(i)} + \frac{\lambda}{m} w_j
$$

Where `j` represents the `j`th feature and m represents the number of training examples.

The algorithm for gradient descent is just like previous posts performing simultaneous updates on `w` and `b` subtracting the gradients $\frac{\partial J(\mathbf{w},b)}{\partial w_j}$ and $\frac{\partial J(\mathbf{w},b)}{\partial b}$ respectively, multiplied by the learning rate alpha.

# Conclusion

That concludes some introduction to the intuition behind why the MSE doesn't work for logistic regression as well as some high level understanding of the math. In the next post I'll be touching on what the dataset looks like as well as a very concise implementation of regularized logistic regression using popular libraries.

# References
* Speech and Language Processing (3rd ed. draft) Dan Jurafsky and James H. Martin. Speech and Language Processing. (n.d.). Retrieved from https://web.stanford.edu/~jurafsky/slp3/ 
* McAdams, J. (2020, January 8). Oranges vs. grapefruit. Kaggle. Retrieved from https://www.kaggle.com/datasets/joshmcadams/oranges-vs-grapefruit 