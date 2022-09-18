---
title: "Mean Squared Error (MSE) for Univariate Linear Regression"
date: 2022-09-16T21:44:05-04:00
draft: false
tags: [
    'Machine Learning',
    'Supervised Learning',
    'Linear Regression'
]
resources:
- name: "featured-image-preview"
  src: "banner.jpg"
---

Andrew Ng covers regression really well in his new course 'Machine Learning Specialization' so I thought it would be helpful to extend
some of his examples in a blog post.

Suppose you're trying to model the relationship between the size of a house in square feet and its market value. Maybe your friend
came to you knowing only the square footage of their home, asking for an accurate estimate of what the property might be worth. Or maybe
they're looking to renovate the home for some extra selling value and are wondering how much more they would receive per square foot.

# Linear Regression

For linear regression, our model in this case would look something like $$f_{w,b}(x^i) = w * x^i + b$$ since we only have one input
feature and are attempting to model a single target variable.

## Mean Squared Error (MSE)

When evaluating the performance of the model, we need an equation to represent the cost. Our cost function J will represent the difference
between our prediction, that is, the output of $f_{w,b}(x^i)$ and the real value of the housing price at $x^i$ for all x values in our training
set. Below is the function J that we will be using to represent the cost with one variable.

$$J(w,b) = \frac{1}{m} \sum\limits_{i = 0}^{m-1} (f_{w,b}(x^{(i)}) - y^{(i)})^2 \tag{1}$$ 
 
- $f_{w,b}(x^{(i)})$ is our prediction for example $i$ using parameters $w,b$.  
- $(f_{w,b}(x^{(i)}) -y^{(i)})^2$ is the squared difference between the target value and the prediction.   
- These differences are summed over all the $m$ examples and divided by `m` to produce the cost, $J(w,b)$.

The above cost function is known as the Mean Squared Error (MSE) since we're computing the squared error across all training examples and
averaging them to result in $J(w,b)$.

In practice, it's often an improvement to use the One Half MSE function as it results in some cleaner values but both functions will work for
the purposes of univariate linear regression.

$$J(w,b) = \frac{1}{2m} \sum\limits_{i = 0}^{m-1} (f_{w,b}(x^{(i)}) - y^{(i)})^2 \tag{2}$$

## Cost Function Intuition

Ultimately, the goal of linear regression is to minimize $J(w,b)$. Since our model $f_{w,b}(x^i)$ predicts the cost of a house
based on the square footage `x`, ultimately our goal would be to find some parameters w and b that results in the smallest cost J.

To illustrate what this means, let's observe what $f_{w,b}(x^i)$ might look like for a fixed b and variable parameter w. The graph on the left
shows some sample data plotted against $f_{w,b}(x)$ and the graph on the right shows how the function $J(w,b)$ varies as w is changed.

![Cost Function 1](intuition-1.png "A small w parameter with fixed b")
![Cost Function 1](intuition-2.png "A large w parameter with fixed b")

## Visualizing $J(w,b)$ in 3D

Now consider having both parameters w and b as variable and plotted on the axies x and y. Imagine a vertical axis z that represents the cost
$J(w,b)$ for any point (w,b) on the plane below it. Visualizing the cost function in this 3D space will result in a convex function (a kind of
bowl shaped graph). And this "bowl shape" would be present for <strong>all</strong> univariate linear regression models.

![Convex Cost](banner.jpg "J(w,b) for all points w and b")