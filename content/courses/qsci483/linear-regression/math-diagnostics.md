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

In this document I will outline the math used to derive and analyze the statistics (calculated quantities) for a linear regression model. Make sure to check out my previous posts before diving in. Recall that linear regression is based upon the equation:
$$
y_i \sim N(X\beta,\sigma^2)
$$
or equivalently 
\begin{align*}
y_i & \sim X\beta + \epsilon_i \\\\
\epsilon_i & \sim N(0,\sigma^2)
\end{align*}
where the $\epsilon_i$ are iid. We have previously derived the estimators $\hat{\beta} = (X'X)^{-1}X'y$, as well as for $\mbox{cov}(\hat{\beta}) = \sigma^2(X'X)^{-1}$.

## Measures of Fit

### The $t$ statistic ###

The most popular statistic used for linear regression is $t\_i = \hat{\beta}\_i/\hat{\sigma}(\hat{\beta}\_i)$. Since we have shown that $\hat{\beta}$ is distributed as a multivariate normal random vector, $\hat{\beta}\_i$ is distributed as a normal random variable. Furthermore, $\hat{\sigma}(\hat{\beta}\_i)$, the standard error for this estimate, is a product of the standard error $\hat{\sigma}(\epsilon)$ and the analytical standard deviation of $\beta_i$ (for $\sigma = 1$) which is just the $\sqrt{(X'X)^{-1}\_{ii}}$. The important thing is we have a normal variable and its standard error, and therefore this statistic is distributed as a $t$ random variable with $(n-k)$ degrees of freedom (recall $n$ is the number of data points and $k$ is the number of predictors).

### The $F$ statistic ###

The $F$ test for the overall model is a formal test with hypothesis:
\begin{align*}
H_0: & ~~~\beta_i = 0 \mbox{ for all } i \\\\
H_1: & ~~~\beta_i \neq 0 \mbox{ for some }i.
\end{align*}
Notably we will be assuming that there _is_ still a non-zero constant mean term even under $H_0$. If we do not assume this then we can basically set all the $\overline{y}$ terms equal to zero and remove the 1 degree of freedom from the null hypothesis terms. To test this we need to calculate a few important quantities, all of which are sums of squares:
- SSE (sum of squares: error)
  - This is basically just the sum of squares for all the residuals
  - SSE~$ = \sum_{i=1}^n (y_i - \hat{y}_i)^2 = \sum_{i=1}^n \epsilon_i^2$
  - We showed [previously](../standard-error) that SSE$\sim \chi^2_{n-k}$
  - If you look back at the derivation you will see that this is true _regardless_ of $\beta$
- SSM (sum of squares: model)
  - This is basically how much extra variation from the mean the model explains
  - SSM~$ = \sum_{i=1}^n (\hat{y}_i - \overline{y})^2$
  - This turns out to _also_ be a $\chi^2$ random variable
  - We can prove this using the same basic argument. Let $J = 11^T$ be a matrix of all 1s. Then: $$\mbox{SSM} = y'(H-\frac{1}{n} J)'(H-\frac{1}{n} J)y.$$ However $H - \frac{1}{n}J$ is symmetric and idempotent, with rank $k-1$. Thus SSM$\sim \chi^2_{k-1}$ following the same logic from [before](../standard-error).
- SST (sum of squares: total)
  - This is how much total variation this is around the mean
  - SST~$ = \sum_{i=1}^n (y_i-\overline{y})^2$
  - This is also $\chi^2_{n-1}$ (prove this yourself?)
Remarkably SSE+SSM=SST, which can be used as an elementary proof that SST is $\chi^2_{n-1}$ (though there are other ways). This is the famous variance decomposition for ANOVA models. Why is this true?

TODO: what is an ANOVA?

#### Variance Decomposition ####

We can think about the variance decomposition several ways. The simplest proof is geometric in nature but requires jumping through some linear-subspace hoops. We can think about three points in $\mathbb{R}^n$, $n$-dimensional space where our datapoints $y$ live. One point is just the data vector $y$. Another point is $\hat{y}\_i = \hat{y}\_i$ a third point is $\overline{y}\_i = \overline{y}$. We can also define $k$ vectors in this space, corresponding to the values of our predictors $x^{(j)}\_i = X\_{ij}$. These $k$ vectors define a $k$-dimensional linear subspace of $\mathbb{R}^n$ (which you can think of as $X\beta$ for all of the possible $k$-dimensional values of $\beta$. The prediction vector $\hat{y}$ is nothing but the projection of $y$ onto the closest point of this subspace. Since one of the predictors is the mean (i.e. $X\_{i1} = 1$) the mean point $\hat{y}$ falls _inside_ this linear subspace. This gives us the following geometric intuition:
- The mean is in the subspace
- The projection is in the subspace
- The projection is the _closest_ point in the subspace to the data. This is because the projection minimizes the sum of squares, or the squared distance between the subspace and the data. 
- Therefore the vector leading from the data to the projection is _orthogonal_ or perpendicular to the subspace
- In particular the vector leading from the data to the projection is orthogonal to the vector leading from the projection to the mean
Therefore we can apply the Pythagorean theorem, to find that the squared distance between the data and the projection (prediction) plus the squared distance between the projection and the mean is equal to the squared distance between the data and the mean. If you sit down and think about what these "squared distance" mean in mathematical terms you will find that this is a geometric proof of SSE+SSM=SST.

We can also think about the variance decomposition in terms of raw linear algebra and matrices: TODO

A common _mistake_ made in deriving the variance decomposition is noting that $$(y_i - \hat{y}_i) + (\hat{y}_i - \overline{y}) = (y_i-\overline{y}).$$ The _false_ argument then goes: square the terms and sum and the equality holds. However, as you likely know $A+B=C$ does _not_ imply that $A^2+B^2=C^2$. We require the additional structure of linear algebra and/or geometry to obtain this result

#### Back to the $F$ test ####

We can now adjust SSE and SSM to get the "mean sum of squares," by dividing by the degrees of freedom:
- MSE = SSE$/(n-k)$
- MSM = SSM$/(n-1)$
Finally, our test statistic is $F = \mbox{MSM}/\mbox{MSE}$. In order for this to be a valid $F$ statistic, SSM and SSE must be independent. This turns out to be true but I will not prove it here (TODO).

Note that if the model explains a lot more variation than just the mean we expect for MSE to be small and MSM to be large: make sure this make sense to you. So if the model is "real" then we expect $F$ to be large. If the null hypothesis is true then we expect $F$ to be small. So we reject the null for large values of $F$. 

### The $R^2$ ###

There are numerous ways to calculate different versions of $R^2$ but at the end of the day they all boil down to correlation coefficients: the correlation $R$ between the predictors and the response variable. We square it because this gives us a measure of the proportion of variance explained by the predictors.

#### Basic $R^2$ ####

The most basic version of this statistic is just the correlation coefficient between the the predictors and the response. It is also known as the coefficient of determination. It is calculated as $SSM/SST$. This gives it the nice interpretation of being the ratio of "variance explained by the model" to "total variance around the mean." It can also be defined mathematically as the square of the Pearson correlation coefficient between $y$ and $\hat{y}$:
\begin{align*}
R^2 & = \left(\frac{\sum_{i=1}^n (y_i-\overline{y})(\hat{y}_i - \overline{y})}{\sqrt{\sum_{i=1}^n (y_i-\overline{y})^2}\sqrt{\sum_{i=1}^n (\hat{y}_i - \overline{y})^2}}\right)^2
\end{align*}

However, the raw $R^2$ calculation has some problems as a measure of model fit. For one thing, $R^2$ _always_ increases whenever you add a new predictor. This is because $\hat{y}$ will always get closer to $y$ as our minimization procedure gains degrees of freedom to find the "best fit." Basically this means that $R^2$ does not penalize overfitting. As we add more and more predictors to our model $R^2$ will just keep getting better and better and will fit coefficients that are highly dependent on the random noise. So we need something better.

#### Adjusted $R^2$ ####

The adjusted $R^2$ tries to account for overfitting by decreasing the original $R^2$ statistic. There are two formulas we can use for the adjusted $R^2$ (which we will denote $\overline{R}^2$):
\begin{align*}
\overline{R}^2
& = 1-(1-R^2)\frac{n-1}{n-k-1} \\\\
& = 1-\frac{\mbox{SSE}/(n-k-1)}{\mbox{SST}/(n-1)}.
\end{align*}
There are two thiings we can note from this. One is we can see how this adjustment works by comparing it to the original formula for $R^2$:
\begin{align*}
R^2 & = \frac{\mbox{SSM}}{\mbox{SST}} \\\\
& = 1 - \frac{\mbox{SSE}}{\mbox{SST}} \\\\
& = 1 - \frac{\mbox{SSE}/n}{\mbox{SST}/n}.
\end{align*}
One way of thinking about $\overline{R}^2$ is that the original (un-adjusted) $R^2$ was using biased estimates $\frac{\mbox{SSE}}{n}$ and $\frac{\mbox{SST}}{n}$ which need to be adjusted for their degrees of freedom $n-k-1$ and $n-1$. TODO(what is SST/n an estimate of).

## Residuals ##

### Standardizing ###
Frequently when evaluating a model we will plot residuals against other things. Often when we do this we will standardize the residuals by dividing $\hat{\epsilon}_i$ by the residual standard error $\hat{\sigma}$. However, this is not necessarily the best way to achieve our goal of truly standardizing the residuals. The issue is that the _residuals_ are different from the _errors_. Although the errors have equal variance, the residuals actually do not. This can be seen by remembering that: $\hat{\epsilon} = M\epsilon$. Thus, in reality the residuals are correlated and have different variances from one another. 

### Studentizing ###
Luckily we can account for this. The way we do this is by "studentizing" the residuals. There are several formulas we can use to represent this, but basically it boils down to the fact that $\hat{\epsilon} \sim N(0,\sigma^2M)$. Thus the true variance of $\hat{\epsilon}\_i$ is not $\sigma^2$ but $\sigma^2M\_{ii}$. Thus we can studentize by calculating
$$t\_i = \frac{\hat{\epsilon}\_i}{\hat{\sigma}(\epsilon)\sqrt{M\_{ii}}}.$$
We can equivalently express this in terms of the [leverage](#leverage) as
$$t\_i = \frac{\hat{\epsilon}\_i}{\hat{\sigma}(\epsilon)\sqrt{1-h\_{ii}}}.$$
TODO: some more on this

### Deleting ###
Since outliers may have outsized influence on the model fit (see the [next section](#measures-of-influence) we may want a more robust statistic for estimating the residuals. We can generate this by considering the _deleted residuals_, or the residuals when comparing a data point to the model fitted _without that data point_. We denote the vector of these as residuals $\hat{y}\_{(i)}$ and a particular residual as $\hat{y}\_{j(i)}$. The deleted residuals are expressed as:
$$d_i = y_i - \hat{y}\_{i(i)}.$$
How can we express these mathematically?

We will start by considering the whole vector $\hat{y}\_{(i)}$. This is the vector of all $n$ predictions, generated using all the data _except_ the $i$-th point. We can re-express $\hat{y}\_{(i)}$ in terms of matrices, although it takes a little thought. Recall that $\hat{y} = Hy = X(X'X)X'y = X\hat{\beta}$. To calculate $\hat{y}\_{(i)}$ we essentially need to first calculate $\hat{\beta}\_{(i)}$, the coefficient estimates without data point $i$. To do this we remove one entry from $y$ and therefore also one row from $X$. However the _dimension_ of $\hat{\beta}$ does not change. We can actually write this as:
$$\hat{\beta}\_{(i)} = (X'X - x_i'x_i)^{-1}(X'y - x_i'y_i).$$
Then to calculate the predictions $\hat{y}\_{(i)}$ we still just multiply by $X$, since we wish to predict all data points, including $y_i$. Thus:
$$\hat{y}\_{(i)} = X(X'X - x_i'x_i)^{-1}(X'y - x_i'y_i).$$
Now we can do a little more algebra to make this nicer, using the very nice [Sherman-Morrison formula](/courses/gaussian-processes/linear-algebra) to calculate
$$(X'X - x_i'x_i)^{-1} = (X'X)^{-1} + \frac{(X'X)^{-1}x_i'x_i(X'X)^{-1}}{1 - x_i(X'X)^{-1}x_i}.$$
The denominator here involves $x_i(X'X)^{-1}x_i$. This is just the leverage $h_{ii}$. We will denote the $n\times 1$ vector representing a column of $H$ as $h_i$. Using this notation, our adjusted predictions become:
\begin{align*}
\hat{y}_{(i)}
& = X\left((X'X)^{-1} + \frac{(X'X)^{-1}x_i'x_i(X'X)^{-1}}{1 - h_{ii}}\right)(X'y - x_i'y_i) \\\\
& = X(X'X)^{-1}X'y - X(X'X)^{-1}x_i'y_i + \frac{1}{1-h_{ii}}X(X'X)^{-1}x_i'x_i(X'X)^{-1}X'y - \frac{1}{1-h_{ii}}X(X'X)^{-1}x_i'x_i(X'X)^{-1}x_i'y_i \\\\
& = Hy - h_iy_i + \frac{1}{1-h_{ii}}h_ih_i'y - \frac{1}{1-h_{ii}}h_ih_{ii}y_i \\\\
& = \hat{y} - \left(1 + \frac{h_{ii}}{1-h_{ii}}\right)h_iy_i + \frac{h_ih_i'y}{1-h_{ii}} \\\\
& = \hat{y} - \frac{h_i(y_i-h_i'y)}{1-h_{ii}} \\\\
& = \hat{y} - \frac{y_i-\hat{y}_i}{1-h_{ii}}h_i \\\\
& = \hat{y} - \frac{\hat{\epsilon}_i}{1-h_{ii}}h_i
\end{align*}
So the $i$th entry of the vector, $\hat{y}\_{i(i)} = \hat{y}_i - \frac{\hat{\epsilon}_ih\_{ii}}{1-h\_{ii}}$. So the deleted residuals become
\begin{align*}
d_i & = y_i - \hat{y}_{i(i)} \\\\
& = y_i - \hat{y}_i - \frac{\hat{\epsilon}_ih_{ii}}{1-h_{ii}} \\
& = \hat{\epsilon_i}\left(1 - \frac{h_{ii}}{1-h_{ii}}\right) \\
& = \frac{1-2h_{ii}}{1-h_{ii}}\hat{\epsilon}_i
\end{align*}

## Measures of Influence ##
One other way of evaluating a regression fit is by considering the influence of each point. An unfortunate side effect of the least squares criterion is that it punishes point that are _very far_ away from the fit more than it punishes points that are _kinda far_ away from the fit. Basically this means that outliers are very influential. We can assess precisely how influential in a variety of ways.

### Leverage ###
One of the most popular ways of assessing this infuence is with the _leverage_. The leverage is defined as a measure of "observation self-sensitivity." It is defined as the partial derivative: $h_{ii} = \dfrac{\partial \hat{y}_i}{\partial y_i}$. That is, how quickly does the predicted value change as the data point changes. This can be easily calculated from our equation for the predicted values: $\hat{y} = Hy$ so that we can see that $h_{ii}$ is in fact just the $ii$-th entry of the hat matrix.

Moreover we can show (TODO) that the leverage is bounded: $0\leq h_{ii}\leq 1$. So the predicted value will always increase when the datapoint increases, but will never increase quite as fast. That fits our intuition that predictions should be consistent with the data, but that no one data point should completely determine the fit.

Because least-squares is so sensitive to outliers, observations with leverages close to 1, or much larger than all the other leverages, should be suspect and might be considered outliers. 

### Cook's Distance ###
Another way of assessing influence is with the Cook's distance. This estimates the effect of deleting a particular observation. It is defined to be $D\_i$: the squared distance between the predictions $\hat{y}$ and the predictions $\hat{y}\_{(i)}$ when observation $i$ is removed from the sample, divided by $k$ times the mean squared error. In math: $$D_i = \frac{\sum_{j= 1}^n (\hat{y}_j - \hat{y}_{j(i)})^2}{k\hat{\sigma^2}}.$$

Therefore the Cook's distance can also be expressed as
\begin{align*}
D_i & = \frac{\sum_{j= 1}^n (\hat{y}_j - \hat{y}_{j(i)})^2}{k\hat{\sigma^2}} \\\\
& = \frac{\left(\frac{\hat{\epsilon}_i}{1-h_{ii}}\right)^2h_i'h_i}{k\hat{\sigma}^2} \\\\
& = \frac{\hat{\epsilon}_i}{k\hat{\sigma}^2}\frac{h_{ii}}{(1-h_{ii})^2}
\end{align*}

### DFFITS ###

A third measure of influence is DFFITS (which may stand for Difference of Fits). It is sort of like a cross between leverage and Cook's distance. Like leverage, it is a measure of self-sensitivity, but like Cook's distance it uses the predictions when a point is left out. The actual mathematical definition is:
$$\mbox{DFFITS}\_i = \frac{\hat{y}\_i - \hat{y}\_{i(i)}}{s\_{(i)}\sqrt{h\_{ii}}},$$
where $\hat{y}\_{i(i)}$ is the predicted value for $y_i$ when $y_i$ is removed from the dataset and $s\_{(i)}$ is the standard error estimated without $y_i$.

Although the formulas for DFFITS and Cook's distance are different, it turns out to be possible to convert from one to another. We will not show this here (TODO) but the general relationship is
$$D_i = \frac{\mbox{DFFITS}^2_i\hat{\sigma^2}_{(i)}}{k\hat{\sigma^2}}.$$
In particular except for very small datasets the mean squared error should not change too much when data point $i$ is remove, so the approximate relationship
$$D_i \approx \frac{\mbox{DFFITS}_i^2}{k}$$
holds. 

## Normality ##

### Moments ###
A common way of quantifying a statistical distribution is with its _moments_. These are defined to be the expectations $E[X], E[X^2], E[X^3], $ and so on ($E[X^i]$ in general). The central moments are defined to be $E[(X-\mu)^i]$ and the standardized moments are $E\left[\left(\frac{X-\mu}{\sigma}\right)^i\right]$. The normal distribution is entirely defined by its first and second moments. In particular its third standardized moment (its _skewness_) is 0 and its fourth standardized moment (its _kurtosis_) is 3. By calculating the sample skewness and sample kurtosis we can heuristically evaluate whether the residuals from our model seem to follow a normal distribution.

### Shapiro-Wilk ###
We can apply a more formal test of normality using the Shapiro-Wilk test. This test statistic is defined by considering the _order statistics_ $x_{(i)}$ from a sample and calculating:
$$W = \frac{\left(\sum_{i=1}^n a_ix_{(i)}\right)^2}{\sum_{i=1}^n (x_i-\overline{x})^2}.$$
The coefficients $a_i$ are obtained as the vector $\frac{V^{-1}m}{C}$ where $m$ is the expected values of the order statistics for iid standard normal random variables, and $V$ is the covariance matrix for those order statistics. $C$ is the length of the vector $V^{-1}m$. There is no classical distribution that fits $W$, so testing is usually done using simulation methods.

## Heteroskedasticity ##

The most common way of testing for heteroskedaticity is the Breusch-Pagan test (implemented in R as ncv.test). This tests whether the variance of the residuals is dependent on the values of the predictors. This tests works by conducting regression as ordinary and calculating the residuals $\hat{\epsilon}$. Then we consider the possibility that the square of the residuals $\hat{\epsilon}^2$ depends upon the predictors. We test this by fitting a new linear model: $$\hat{\epsilon}^2 = X\gamma + \eta.$$
The test statistic is $nR^2$ where $R^2$ is the coefficient of determination for the second model. Under the null hypothesis that the predictors have no effect. this test statistic is distributed $\chi^2_{k-1}$. (TODO: why?)