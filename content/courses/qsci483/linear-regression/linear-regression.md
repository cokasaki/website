---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: General Model
menu:
  qsci483:
    parent: Linear Regression
    weight: 3
title: Linear Regression
toc: true
type: docs
weight: 3
---

In this document I will outline the math used in the analysis of a more general linear regression model, with more than one predictor variable. Make sure to check out my [notation](/courses/qsci483/notation) document to clarify any confusing notation, and check out my [simple linear regression](/courses/qsci483/linear-regression/simple-linear-regression) document if you would like a slightly simpler analysis to start off with. This document will charge right into matrix notation from the start. 

## The Model ##

Linear regression is based upon the equation
$$
y_i \sim N(X\beta,\sigma^2)
$$
or equivalently 
\begin{align*}
y_i & \sim X\beta + \epsilon_i \\\\
\epsilon_i & \sim N(0,\sigma^2)
\end{align*}
where the $\epsilon_i$ are iid. In the context of regression, it is important to remember that: $E[y_i] = X\beta$ and $E[\epsilon_i] = 0$. We will assume that there are $n$ data points and $k$ predictors. This $\beta$ is a $k\times 1$ vector, $X$ is a $n\times k$ matrix and $y$ and $\epsilon$ are $n\times 1$ vectors. 

### Fitting ###
#### Objective Function ####
To fit a simple linear regression we choose a measure of fit: least squares. This means we are looking for the parameter estimates that minimize the sum of squared residuals. A residual is calculated by the difference between $y_i$ and $\hat{y}_i$, the value predicted by our fitted model. We can calculate $\hat{y}_i$ using the $1\times k$ vector $x_i$ of predictors for data point $i$:
$$
\hat{y}_i = x_i\hat{\beta}.
$$
Alternatively we can find the whole vector $\hat{y} = X\beta$. To find a single squared residuals we calculate $r_i^2 = (y_i - x_i\hat{\beta})^2$. We will define the function $f(\hat{\beta})$ to be the sum of squared residuals, as a function of the fitted coefficients. We want to find the coefficients which minimize this function. We can calculate this as:
\begin{align*}
f(\hat{\beta}) 
& = \sum_{i=1}^n (\mbox{residual})^2 \\\\
& = \sum_{i=1}^n (y_i - x_i\hat{\beta})^2 \\\\
& = \sum_{i=1}^n \left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)^2
\end{align*}
In matrix notation we can rewrite this, however. The residual _vector_ can be written as the $n\times 1$ vector
$$
r = y - X\hat{\beta}.
$$
The sum of squares can then be written as $r'r$. So then
\begin{align*}
f(\hat{\beta})
& = r'r \\\\
& = (y-X\hat{\beta})'(y-X\hat{\beta}).
\end{align*}
We can expand this quadratic equation as
\begin{align*}
f(\hat{\beta})
& = y'y - y'X\hat{\beta} - \hat{\beta}'X'y + \hat{\beta}'X'X\hat{\beta}.
\end{align*}

#### Optimization ####
From calculus, we can use the fact that the extrema (minimums and maximums) of a function occur when the partial derivatives of that function are equal to zero. So we want to take the partial derivatives of $f$ with respect to each $\beta_i$, to find the minimum sum of squared residuals (the _least squares_). So let us take the partial derivative with respect to a particular coefficient:
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}_j} 
& = \dfrac{\partial }{ \partial \hat{\beta}_j} \sum_{i=1}^n \left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)^2 \\\\
& = \sum_{i=1}^n \dfrac{\partial }{ \partial \hat{\beta}_j}\left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)^2 \\\\
& = \sum_{i=1}^n 2\left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)\dfrac{\partial }{ \partial \hat{\beta}_j}\left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right) \\\\
& = \sum_{i=1}^n 2\left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)\left(-x_{ij}\right) \\\\
& = -\sum_{i=1}^n 2x_{ij}\left(y_i - \sum_{j=1}^k x_{ij}\hat{\beta}_j\right)\end{align*}
Verify that this can be written in matrix notation as
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}_j}
& = -2\left(X'y - X'X\hat{\beta}\right)_j,
\end{align*}
or in matrix calculus notation
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}} & = -2\left(X'y - X'X\hat{\beta}\right).
\end{align*}
Remember that according to calculus, every single entry $\dfrac{\partial f}{ \partial \beta_j}$ must be equal to zero at any extremum (and in particular at the minimum). Thus we can set this whole matrix equation equal to zero, and we get
$$
X'y = X'X\hat{\beta}.
$$
Since we are trying to derive an equation for $\hat{\beta}$ we move the matrix over to the other side and we get
$$
\hat{\beta} = (X'X)^{-1}X'y.
$$
We can derive this same equation in fewer steps using the more complex matrix calculus notation, which for example allows us to take the derivative of matrix products and use the matrix chain rule:
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}}
& = \dfrac{\partial }{ \partial \hat{\beta}}\left(y'y - y'X\hat{\beta} - \hat{\beta}'X'y + \hat{\beta}'X'X\hat{\beta}\right) \\\\
& = - \dfrac{\partial }{ \partial \hat{\beta}}\left(y'X\hat{\beta}\right) - \dfrac{\partial }{ \partial \hat{\beta}}\left(\hat{\beta}'X'y\right) + \dfrac{\partial }{ \partial \hat{\beta}}\left(\hat{\beta}'X'X\hat{\beta}\right) \\\\
& = -y'X-(X'y)'+\hat{\beta}'X'X + (X'X\hat{\beta})' \\\\
& = -2(X'y - X'X\hat{\beta})' \\\\
& = 0.
\end{align*}

### Important Quantities

Now we have shown that the ordinary least squares estimator for $\beta$ is $(X'X)^{-1}X'y$. Using this we may calculate all sorts of other things.

#### Predicted Values ####
The predicted values are
\begin{align*}
\hat{y}
& = X\hat{\beta} \\\\
& = X(X'X)^{-1}X'y \\\\
& = Hy.
\end{align*}
Here we have defined the "hat matrix," $H = X(X'X)^{-1}X'$. This is a useful matrix in linear regression, which maps the data to its predicted values. This is sometimes also called the projection matrix $P$, since it projects the data onto a lower-dimensional linear space. It is sometimes also called the influence matrix. It has two nice properties which we will use in a moment: it is symmetric and idempotent. This means $H'=H$ and $H^2 = H'H = H$. We can see this by calculating:
\begin{align*}
H' & = (X(X'X)^{-1}X')' \\\\
& = X(X'X)^{-1}X' \\\\
& = H \\\\
H^2 & = (X(X'X)^{-1}X')(X(X'X)^{-1}X') \\\\
& = X(X'X)^{-1}(X'X)(X'X)^{-1}X' \\\\
& = X(X'X)^{-1}X' \\\\
& = H
\end{align*}

#### Residuals ####
The residuals are
\begin{align*}
\hat{\epsilon}
& = y-\hat{y} \\\\
& = y-Hy \\\\
& = (I-H)y \\\\
& = (I-X(X'X)^{-1}X')y \\\\
& = My
\end{align*}
Here we have defined the "residual maker" matrix, which can also be called the residual operator. This matrix takes the data and gives you the residuals of the model. It also inherits symmetry and idempotency from the hat matrix, since:
\begin{align*}
M' & = (I-H)' \\\\
   & = I-H \\\\
M^2 & = (I-H)(I-H) \\\\
& = I - 2H + H^2 \\\\
& = I - 2H + H \\\\
& = I-H
\end{align*}
With all of this put together we now have the tools to analyze the [residual standard error](../standard-error). First, however, we will analyze the properties of $\hat{\beta}$. 
