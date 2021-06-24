---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: Properties
menu:
  qsci483:
    parent: Linear Regression
    weight: 5
title: Properties of Linear Regression
toc: true
type: docs
weight: 5
---

In this document I will outline the math used to analyze our previous results for linear regression analysis. Make sure to check out my previous posts on [notation](/courses/qsci483/notation) and [simple linear regression](/courses/qsci483/linear-regression/linear-regression) before diving in. 

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
where the $\epsilon_i$ are iid. We have previously found that the estimate $\hat{\beta} = (X'X)^{-1}Xy$ minimizes the sum of squares. In this document we will show: (1) that $\hat{\beta}$ is also the maximum likelihood estimator, (2) that $\hat{\beta}$ is unbiased, and (3) the covariance matrix for the estimator $\hat{\beta}$.

### Maximum Likelihood ###

Our probability model has that $y_i$ are normally distributed, and are independent of each other given the predictors $X$ and the coefficients $\beta$. The _likelihood function_ is given by the probability (density) of the data given the parameters ($\beta$), expressed as a function of the parameter estimate ($\hat{\beta}$). This function in our case can be written as a product of Gaussian (normal) probability density functions (pdfs):
\begin{align*}
\ell(\hat{\beta}) & = \prod_{i=1}^n N(y_i|X\hat{\beta},\sigma^2) \\\\
& = \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(y_i - X\hat{\beta})}{2\sigma^2}\right) \\\\
& = \frac{1}{(2\pi\sigma^2)^{n/2}}\exp\left(-\sum_{i=1}^n \frac{(y_i - X\hat{\beta})}{2\sigma^2}\right).
\end{align*}
We will now make use of a common trick in statistics: we will calculate the log-likelihood function $\lambda(\hat{\beta}) = \log(\ell(\hat{\beta}))$. Since the probability density function is always non-negative, and therefore the likelihood is always non-negative, the log-likelihood can be defined. Furthermore, the log is a convex function, and because of this it has the property that $\ell$ and $\lambda$ are minimized at the same value $\hat{\beta}$. Why this is is not important for this class.

So taking the log we obtain
\begin{align*}
\lambda(\hat{\beta})
& = \log\left(\frac{1}{(2\pi\sigma^2)^{n/2}}\exp\left(-\sum_{i=1}^n \frac{(y_i - X\hat{\beta})}{2\sigma^2}\right)\right) \\\\
& = \log\left(\frac{1}{(2\pi\sigma^2)^{n/2}}\right) + \log\left(\exp\left(-\sum_{i=1}^n \frac{(y_i - X\hat{\beta})}{2\sigma^2}\right)\right) \\\\
& = -\frac{n}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{i=1}^n (y_i-X\hat{\beta})^2.
\end{align*}
Now remember that we are looking for the _maximum likelihood estimator_. So we want to find the value of $\hat{\beta}$ which maximizes the likelihood (i.e. maximizes the probability of that data, given the parameters). Viewing this as a function of $\hat{\beta}$ we see that maximizing $\lambda$ is equivalent to minimizing
$$
\sum_{i=1}^n (y_i-X\hat{\beta})^2.
$$
Therefore the maximum likelihood estimator (MLE) of $\hat{\beta}$ is exactly the least-squares estimator $\hat{\beta} = (X'X)^{-1}X'y$. Importantly, we are at this point omitting the parameter $\sigma^2$. In fact, the MLE for $\hat{\beta}$ is unchanged if we estimate this parameter as well. The MLE for $\hat{\sigma^2}$ is given by
\begin{align*}
\hat{\sigma^2}
& = \frac{1}{n}\sum_{i=1}^n (y_i - \hat{y}_i)^2 \\\\
& = \frac{1}{n}y'(I-X(X'X)^{-1}X')y.
\end{align*}

### Unbiasedness ###

It is important to remember that estimators (such as $\hat{\beta}) are themselves _random_. If we were to simulate from our model, holding $\beta$ fixed, we would fit a different estimator $\hat{\beta}$ to every simulation. Thus an important quality that an estimator can have is _unbiasedness_. This means that, if the model is correct, the estimator will neither tend to overestimate nor underestimate the true parameter. In mathematical terms, its expectation (or mean) is correct: $E[\hat{\beta}] = \beta$.

Using our matrix math this property can be derived fairly quickly. We know that $\hat{\beta} = (X'X)^{-1}X'y$. We also know that $y = X\beta + \epsilon$. Putting these together we see:
\begin{align*}
\hat{\beta}
& = (X'X)^{-1}X'(X\beta + \epsilon) \\\\
& = (X'X)^{-1}X'X\beta + (X'X)^{-1}X'\epsilon \\\\
& = \beta + (X'X)^{-1}X'\epsilon.
\end{align*}
Therefore the mean is
\begin{align*}
E[\hat{\beta}]
& = E\left[\beta + (X'X)^{-1}X'\epsilon\right].
\end{align*}
The mean has a useful property which we will use here, which is that it is _linear_. This means that the expectation of a sum is the sum of the expectations. In math we write this as $E[A+B] = E[A] + E[B]$. Since matrix operations are essentially just a bunch of sums, we can also write $E[Mv] = ME[v]$, if $v$ is random and $M$ is constant. Using this property we get
\begin{align*}
E[\hat{\beta}]
& = E\left[\beta + (X'X)^{-1}X'\epsilon\right] \\\\
& = \beta + E\left[(X'X)^{-1}X'\epsilon\right] \\\\
& = \beta + (X'X)^{-1}X'E[\epsilon] \\\\
& = \beta
\end{align*}
since we have assumed that $\epsilon$ is zero-mean noise. So, in fact, our estimator is unbiased!

### Covariance of $\hat{\beta}$ ###

Remember that $\hat{\beta}$ is fundamentally a _random_ quantity. It is different in every realization (or simulation) of the statistical model we have written down. It is sensitive to the addition of random noise ($\epsilon_{i}$) to the data. Luckily, since we have written down a statistical model for our data $y$ we are able to do a statistical analysis for the estimator $\hat{\beta}$ to determine its random properties.

We showed in the previous section that $\hat{\beta}$ was unbiased, that is, it has its mean $E[\hat{\beta}] = \beta$ at the correct place. We will now calculate the variance-covariance (or just covariance) matrix of $\hat{\beta}$. This tells us how much we can expect $\hat{\beta}$ to vary for different datasets. The _diagonal_ of this covariance matrix tells us the variances of $\hat{\beta}\_{i}$, which are important quantities that get used, for example, in calculating Rs model summaries.


The covariance of two random variables is given by $E[(A-E[A])(B-E[B])]$. In our case $A = \hat{\beta}\_{i}$ and $B = \hat{\beta}\_{j}$. This will give us the entry $\Sigma_{ij}$ in the covariance matrix. We already know that $E[\hat{\beta}_{i}] = \beta_{i}$ since the estimators are unbiased. Therefore the covariance is 
$$
\Sigma_{ij} = E\left[(\hat{\beta}_{i} - \beta_{i})(\hat{\beta}_{j} - \beta_{j})\right].
$$
Expanding the quadratic in the middle, and remembering that expectations are linear, we get that
\begin{align*}
\Sigma_{ij}
& = E[\hat{\beta}_i\hat{\beta}_j] - E[\beta_i\hat{\beta}_j] - E[\hat{\beta}_i\beta_j] + E[\beta_i\beta_j] \\\\
& = E[\hat{\beta}_i\hat{\beta}_j] - \beta_iE[\hat{\beta}_j] - E[\hat{\beta}_i]\beta_j + \beta_i\beta_j \\\\
& = E[\hat{\beta}_i\hat{\beta}_j] - \beta_i\beta_j.
\end{align*}


This is a useful formula, but it would be far more effective to analyze this problem in terms of matrix notation. Let's go ahead and make that switch. Using matrix notation, the single entry
$$
\Sigma_{ij} = E\left[(\hat{\beta}_i - \beta_i)(\hat{\beta}_j - \beta_j)\right].
$$
can be written as a whole matrix
$$
\Sigma = E\left[(\hat{\beta} - E[\hat{\beta}])(\hat{\beta} - E[\hat{\beta}])'\right].
$$
Since we know that the estimator is unbiased we can plug this into the matrix equation to get
\begin{align*}
\Sigma
& = E\left[(\hat{\beta} - \beta)(\hat{\beta}-\beta)'\right] \\\\
& = E\left[\hat{\beta}\hat{\beta}'\right] - \beta\beta'.
\end{align*}
This is exactly the same formula we have above, just written in matrix notation. Now, we already have a formula for $\hat{\beta}$ in matrix notation, $\hat{\beta} = (X'X)^{-1}X'y$, so let's plug this in here. We get
\begin{align*}
\Sigma
& = E\left[(X'X)^{-1}X'yy'X(X'X)^{-1}\right] - \beta\beta' \\\\
& = (X'X)^{-1}X' E[yy'] X(X'X)^{-1} - \beta\beta'.
\end{align*}
Now we can plug in our formula for $y = X\beta + \epsilon$. Remembering that expectations are linear (i.e. can be split up over summations), we get
\begin{align*}
E[yy']
& = E\left[(X\beta + \epsilon)(X\beta+\epsilon)'\right] \\\\
& = E\left[X\beta\beta'X' + \epsilon X\beta + \beta'X'\epsilon + \epsilon\epsilon'\right] \\\\
& = X\beta\beta'X' + E[\epsilon]X\beta + \beta'X'E[\epsilon] + E[\epsilon\epsilon'] 
\end{align*}
Now we already know that $E[\epsilon] = 0$. What about $E[\epsilon\epsilon']$? Well,
$$
\mbox{cov}(\epsilon_i,\epsilon_j) = E[\epsilon_i\epsilon_j] - E[\epsilon_i]E[\epsilon_j] = E[\epsilon_i\epsilon_j].
$$
Since independent variables are uncorrelated (and we know that the $\epsilon_i$ are iid) we see that this matrix is diagonal with entries equal to $\sigma^2$, the variance of $\epsilon_i$. Thus:
$$
E[yy'] = X\beta\beta'X' + \sigma^2I.
$$
Finally, plugging this in, we can find that
\begin{align*}
\Sigma & = (X'X)^{-1}X' E[yy'] X(X'X)^{-1} - \beta\beta' \\\\
& = (X'X)^{-1}X'(X\beta\beta'X' + \sigma^2I)X(X'X)^{-1} - \beta\beta' \\\\
& = (X'X)^{-1}(X'X)\beta\beta'(X'X)(X'X)^{-1} + (X'X)^{-1}X'(\sigma^2I)X(X'X)^{-1} - \beta\beta' \\\\
& = \beta\beta' + \sigma^2(X'X)^{-1} - \beta\beta' \\\\
& = \sigma^2(X'X)^{-1}.
\end{align*}
This is the covariance matrix of $\hat{\beta}$! Notably, this has two important properties. One, it depends on $\sigma^2$, so if we want to use this we had better estimate $\sigma^2$ somehow. More on this in the next section. Two, it depends on the _inverse_ of $X'X$. There's no guarantee that this matrix is invertible. For example, if we have more predictors than we have data points (i.e. $k > n$) this matrix will certainly _not_ be invertible. You may have been warned about this scenario in the past. However, even if you have many data points, other situations can crop up where $X'X$ is either numerically difficult to invert (i.e. difficult to calculate on a computer), or produces very large variances. One such case is where two of the predictor variables are very highly correlated. More on this later.

### Other Quantities ###

Here we will show briefly that $\hat{y}$ is also unbiased (assuming the model is correct). This can be seen by some matrix calculations:
\begin{align*}
E[\hat{y}]
& = E[Hy] \\\\
& = E[X(X'X)^{-1}X'(X\beta+\epsilon)] \\\\
& = E[X\beta + H\epsilon] \\\\
& = X\beta + HE[\epsilon] \\\\
& = X\beta.
\end{align*}
Meanwhile, $\hat{\epsilon}$ is:
\begin{align*}
\hat{\epsilon}
& = My \\\\
& = (I-H)y \\\\
& = y - (X\beta + H\epsilon) \\\\
& = (I-H)\epsilon \\\\
& = M\epsilon
\end{align*}
This is unbiased in the sense that it has the same mean as $\epsilon$. Unfortunately, although it might seem we can simply calculate $\epsilon = M^{-1}\hat{\epsilon}$, this matrix may not be invertible (TODO: I'm like 99% certain it is always uninvertible)