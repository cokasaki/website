---
date: "2020-04-27T00:00:00+01:00"
draft: false
linktitle: Descriptive Statistics
menu:
  qsci483:
    parent: Point Processes
    weight: 3
title: Descriptive Statistics
toc: true
type: docs
weight: 3
---

In this section I will outline the descriptive statistics that are used to diagnose point process deviations from homogeneity. A standard assumption in the analysis of point processes is that equal areas should have equal densities of points. However, this assumption is often violated. We can diagnose such violations using descriptive statistics such as Ripley's $K$ and $L$ functions.

### Ripley's $K$ function ###

This function is defined as
$$
\hat{K}(t) = \lambda^{-1}\sum_{i\neq j} \frac{I(d_{ij} < t)}{n}.
$$

Here $\lambda$ is the averasge intensity of the point process, which we will usually estimate as $n/A$ where $n$ is the ttoal number of points and $A$ is the area in question. $I$ is the indicator function which is 1 if the argument is true and 0 if it is false. The summation can be thought of as calculating the fraction of all pairs which are within $t$ units of distance from each other. If points are homogeneous in two dimensions then, ignoring boundary effects, we can expect that each point will contain approximately $\lambda \pi t^2$ points within $t$ distance units of it. So we should expect $\hat{K}(t) \approx \pi t^2$.

### Ripley's $L$ function ###

The $K$ function suffers from non-constant variance, which can make analyses difficult. Instead, we often use Ripley's $L$ function, defined to be

$$
\hat{L}(t) = \sqrt{\frac{\hat{K}(t)}{\pi}}.
$$

This function has mean $t$ and variance approximately constant for all $t$. We can interpret plots of Ripley's $L$ function as follows. First, we expect that the $L$ function should follow a straight line through the origin with slope 1. If the $L$ function deviates too much we conclude that there is clustering or overdispersion at that relevant length scale. If the $L$ function deviates to the left of the line, then $\hat{K}(t)$ is larger than expected, meaning more points fall within distance $t$ than expected. Thus leftward deviations denote clustered processes. Conversely, if the $L$ function is to the right of the line then the points are overdispersed.
