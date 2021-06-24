---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: Simple Model
menu:
  qsci483:
    parent: Linear Regression
    weight: 2
title: Simple Linear Regression
toc: true
type: docs
weight: 2
---

In this document I will outline the math used in the analysis of a simple linear regression model. Make sure to check out my [notation](/courses/qsci483/math/notation) document to clarify any confusing notation. 

## The Model ##

Simple Linear Regression is based upon the equation
$$
y_i \sim N(\beta_0 + \beta_1 x_i,\sigma^2)
$$
or equivalently 
\begin{align*}
y_i & \sim \beta_0 + \beta_1 x_i + \epsilon_i \\\\
\epsilon_i & \sim N(0,\sigma^2)
\end{align*}
where the $\epsilon_i$ are iid. It is important to remember that the expectations, or means, of these variables are: $E[y_i] = \beta_0 + \beta_1 x_i$ and $E[\epsilon_i] = 0$. We will assume that there are $n$ data points and only 1 predictor. This is what makes it _simple_ linear regression. In more general linear regression models you have more than 1 predictor. In multivariate linear regression models you have more than one dependent variable as well. In addition to the predictor variable, we also have an intercept, or mean term, which can be thought of as a second predictor.

### Fitting ###
#### Objective Function ####
To fit a simple linear regression we first must choose a measure of fit. Here we will choose least squares, since this corresponds to [maximum likelihood esitmation](/courses/qsci483/math/linear-regression/properties) in this model. This means we are looking for the parameter estimates that minimize the sum of squared residuals. A residual is calculated by the difference between $y_i$ and $\hat{y}_i$, the value predicted by our fitted model. We can calculate $\hat{y}_i$ using our predictor $x_i$ for data point $i$ along with estimated coefficients $\hat{\beta}_0$ and $\hat{\beta}_1$:
$$
\hat{y}_i = \hat{\beta}_0 + \hat{\beta}_1 x.
$$
Alternatively we can find the whole vector $\hat{y} = \hat{\beta_0}\mathbf{1} + x\hat{\beta}$ (where $\mathbf{1}$ is a vector of all ones). To find a single squared residuals we calculate $r_i^2 = (y_i - \hat{\beta}_0 - \hat{\beta}_1 x)^2$. We will define the function $f(\hat{\beta}_0,\hat{\beta}_1)$ to be the sum of squared residuals, as a function of the fitted coefficients. We want to find the coefficients which minimize this function. We can calculate this as:
\begin{align*}
f(\hat{\beta}) 
& = \sum_{i=1}^n (\mbox{residual})^2 \\\\
& = \sum_{i=1}^n (y_i - \hat{\beta}_0 - \hat{\beta}_1)^2 \\\\
\end{align*}

#### Optimization ####
From calculus, we can use the fact that the extrema (minimums and maximums) of a function occur when the partial derivatives of that function are equal to zero. So we want to take the partial derivatives of $f$ with respect to both $\beta_0$ and $\beta_1$, to find the minimum sum of squared residuals (the _least squares_). So let us take the partial derivative with respect to $\hat{\beta}_0$:
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}_0} }
& = \dfrac{\partial }{ \partial \hat{\beta}_0} \sum_{i=1}^n \left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i)^2 \\\\
& = \sum_{i=1}^n \dfrac{\partial }{ \partial \hat{\beta}_0}\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i)^2 \\\\
& = \sum_{i=1}^n 2\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right)\dfrac{\partial }{ \partial \hat{\beta}_0}\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right) \\\\
& = -\sum_{i=1}^n 2\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right)
\end{align*}
To set this derivative equal to zero we need to set:
\begin{align*}
\sum_{i=1}^n \left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right) & = 0.
\end{align*}
This can be accomplished by splitting up the sum and getting
\begin{align*}
\sum_{i=1}^n y_i & = \sum_{i=1}^n \hat{\beta}_0 + \sum_{i=1}^n \hat{\beta}_1 x_i \\\\
n\hat{\beta}_0 & = \sum_{i=1}^n y_i - \hat{\beta}_1 \sum_{i=1}^n x_i \\\\
\hat{\beta}_0 & = \overline{y} - \hat{\beta}_1\overline{x}
\end{align*}
Now that we have calculated $\hat{\beta}_0$ in terms of $y,x,$ and $\hat{\beta_1}$ we can take the derivative with respect to $\beta_1$ to find the least squares estimator:
\begin{align*}
\dfrac{\partial f}{ \partial \hat{\beta}_1} }
& = \dfrac{\partial }{ \partial \hat{\beta}_1} \sum_{i=1}^n \left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i)^2 \\\\
& = \sum_{i=1}^n \dfrac{\partial }{ \partial \hat{\beta}_1}\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i)^2 \\\\
& = \sum_{i=1}^n 2\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right)\dfrac{\partial }{ \partial \hat{\beta}_1}\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right) \\\\
& = -\sum_{i=1}^n 2\left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right)(x_i)
\end{align*}
To set this derivative equal to zero we need to set:
\begin{align*}
\sum_{i=1}^n \left(y_i - \hat{\beta}_0 - \hat{\beta}_1x_i\right)x_i & = 0.
\end{align*}
But here we can plug in our estimator $\hat{\beta}_0$ to get
\begin{align*}
\sum_{i=1}^n \left(y_i - (\overline{y}-\hat{\beta}_1\overline{x}) - \hat{\beta}_1x_i\right)x_i & = 0.
\end{align*}
Then we can expand the sum on the left to get
\begin{align*}
\sum_{i=1}^n (y_i - \overline{y})x_i - \sum_{i=1}^n \hat{\beta}_1(x_i - \overline{x})x_i & = 0 \\\\
\hat{\beta}_1 \sum_{i=1}^n (x_i - \overline{x})x_i & = \sum_{i=1}^n (y_i-\overline{y})x_i \\\\
\hat{\beta}_1 & = \frac{\sum_{i=1}^n (y_i-\overline{y})x_i}{\sum_{i=1}^n (x_i - \overline{x})x_i}
\end{align*}
So we have found an estimator for $\hat{\beta}_1$ in terms of only the predictors and the responses. We also have an estimator for $\hat{\beta}_0$ in terms of the predictors, the responses, and $\hat{\beta}_1$. Now, traditionally, $\hat{\beta}_1$ is written in a slightly different form, as
\begin{align*}
\hat{\beta}_1 & = \frac{S_{xy}}{S_{xx}} \\\\
S_{xx} & = \frac{1}{n-1}\sum (x_i-\overline{x})^2 \\\\
S_{xy} & = \frac{1}{n-1}\sum (x_i-\overline{x})(y_i-\overline{y}).
\end{align*}
The reason for this is that $S_{xy}$ is the sample covariance of $x$ and $y$, and $S_{xx}$ is the sample variance of $x$, so it is nice to express $\hat{\beta}_1$ in terms of other statistics that we already know about. We can see that the two formulas for $\hat{\beta}_1$ are equivalent by doing a little more math, taking $S_{xx}$ and $S_{xy}$ and changing them to a slightly different format:
\begin{align*}
(N-1)S_{xy} & = \sum (x_i-\overline{x})(y_i-\overline{y}) \\\\
& = \sum (y_i-\overline{y})x_i - \sum (y_i-\overline{y})\overline{x} \\\\
& = \sum (y_i-\overline{y})x_i - \overline{x}\sum (y_i - \overline{y}) \\\\
& = \sum (y_i -\overline{y})x_i - \overline{x}(\sum \left(y_i - \frac{1}{n}\sum y_i\right) \\\\
& = \sum (y_i -\overline{y})x_i - \overline{x}(\sum y_i - \sum y_i) \\\\
& = \sum (y_i - \overline{y})x_i
\end{align*}
I encourage you to try doing the same calculation with $S_{xx}$: you will find that it follows exactly the same format. So we can see that the formula we have derived for $\hat{\beta}_1$ is exactly the same as the traditional format in terms of the sample (co)variances. 