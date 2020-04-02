---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: Model Diagnostics
menu:
  qsci483:
    parent: Linear Regression
    weight: 6
title: Linear Regression Model Diagnostics
toc: true
type: docs
weight: 6
---

In this document I will outline the math used to derive and analyze the model diagnostics for a linear regression model. Make sure to check out my previous posts before diving in. 

## The Model ##

Recall that linear regression is based upon the equation:
$$
y_i \sim N(X\beta,\sigma^2)
$$
or equivalently 
\begin{align*}
y_i & \sim X\beta + \epsilon_i \\\\
\epsilon_i & \sim N(0,\sigma^2)
\end{align*}
where the $\epsilon_i$ are iid. We have previously derived the estimators $\hat{\beta} = (X'X)^{-1}X'y$, as well as for $\mbox{cov}(\hat{\beta}) = \sigma^2(X'X)^{-1}$.

### The $t$ statistic ###

The most popular statistic used for linear regression is $t_i = \hat{\beta}_i/\hat{\sigma}(\hat{\beta}_i)}$. Since we have shown that $\hat{\beta}$ is distributed as a multivariate normal random vector, $\hat{\beta}_i$ is distributed as a normal random variable. Furthermore, $\hat{\sigma}(\hat{\beta}_i)$, the standard error for this estimate, is a product of the standard error $\hat{\sigma}(\epsilon)$ and the analytical standard deviation of $\beta_i$ (for $\sigma = 1$) which is just the $\sqrt{(X'X)^{-1}_{ii}}$. The important thing is we have a normal variable and its standard error, and therefore this statistic is distributed as a $t$ random variable with $(n-k)$ degrees of freedom (recall $n$ is the number of data points and $k$ is the number of predictors).

### The $F$ statistic ###

The $F$ test for the overall model is a formal test with hypothesis:
\begin{align*}
H_0: & \beta_i = 0 \mbox{ for all } i \\\\
H_1: & \beta_i \neq 0 \mbox{ for some }i.
\end{align*}
Notably we will be assuming that there _is_ still a non-zero constant mean term even under $H_0$. If we do not assume this then we can basically set all the $\overline{y}$ terms equal to zero and remove the 1 degree of freedom from the null hypothesis terms. To test this we need to calculate a few important quantities, all of which are sums of squares:
- SSE (sum of squares: error)
  - This is basically just the sum of squares for all the residuals
  - SSE$ = \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \sum_{i=1}^n \epsilon_i^2$
  - We showed [previously](../standard-error) that SSE$\sim \chi^2_{n-k}$
- SSM (sum of squares: model)
  - This is basically how much extra variation from the mean the model explains
  - SSM$ = \sum_{i=1}^n (\hat{y}_i - \overline{y})^2
- SST (sum of squares: total)
  - This is how much total variation this is around the mean
  - SST$ = \sum_{i=1}^n (y_i-\overline{y})^2
Remarkably SSE+SSM=SST. This is the famous variance decomposition for ANOVA models. Why is this true?

#### Variance Decomposition ####

We can think about the variance decomposition several ways. The simplest proof is geometric in nature but requires jumping through some linear-subspace hoops. We can think about three points in $\mathbb{R}^n$, $n-dimensional space where our datapoints $y$ live. One point is $y_i = (y_i)$. Another point is $\hat{y}_i = (\hat{y}_i)$ a third point is $\overline{y}_i = (\overline{y})$. We can also define $k$ vectors in this space, corresponding to the values of our predictors $x^{(j)}_i = X_{ij}$. These $k$ vectors define a $k$-dimensional linear subspace of $\mathbb{R}^n$ (which you can think of as $X\beta$ for all of the possible $k$-dimensional values of $\beta$. The prediction vector $\hat{y}$ is nothing but the projection of $y$ onto the closest point of this subspace. Since one of the predictors is the mean (i.e. $X_{i1} = 1$) the mean point $\hat{y}$ falls _inside_ this linear subspace. This gives us the following geometric intuition:
- The mean is in the subspace
- The projection is in the subspace
- The projection is the _closest_ point in the subspace to the data
- Therefore the vector leading from the data to the projection is _orthogonal_ or perpendicular to the subspace
- In particular the vector leading from the data to the projection is orthogonal to the vector leading from the projection to the mean
Therefore we can apply the Pythagorean theorem, to find that the squared distance between the data and the projection (prediction) plus the squared distance between the projection and the mean is equal to the squared distance between the data and the mean. If you sit down and think about what these "squared distance" mean in mathematical terms you will find that this is a geometric proof of SSE+SSM=SST.

We can also think about the variance decomposition in terms of raw linear algebra and matrices: TODO

A common _mistake_ made in deriving the variance decomposition is noting that $$(y_i - \hat{y}_i) + (\hat{y}_i - \overline{y}) = (y_i-\overline{y}).$$ The _false_ argument then goes: square the terms and sum and the equality holds. However, as you likely know $A+B=C$ does _not_ imply that $A^2+B^2=C^2$. We require the additional structure of linear algebra and/or geometry to obtain this result

#### Back to the $F$ test ####

We can now adjust SSE and SSM to get the "mean sum of squares," by dividing by the degrees of freedom:
- MSE = SSE/$(n-k)$
- MSM = SSM/$(n-1)$
Finally, our test statistic is $F = MSM/MSE$. Note that if the model explains a lot more variation than just the mean we expect for MSE to be small and MSM to be large: make sure this make sense to you. So if the model is "real" then we expect $F$ to be large. If the null hypothesis is true then we expect $F$ to be small. So we reject the null for large values of $F$. 

Multiple vs. adjusted $R^2$
More on t-tests
F-test for whole model
Stundentized residuals ($t$ vs. standardized $N(0,1)$)
DFFITS
Cook's distance ($F$-distribution)
Skewness
Kurtosis
Leverage
ANOVA
Shapiro-wilks test
ncV test