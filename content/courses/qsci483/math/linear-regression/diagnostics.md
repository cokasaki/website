---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: Model Diagnostics
menu:
  qsci483:
    parent: Linear Regression
    weight: 5
title: Linear Regression Model Diagnostics
toc: true
type: docs
weight: 5
---

In this document I will outline the math used to derive and analyze the model diagnostics for a simple linear regression model. Make sure to check out my previous posts on [notation](/courses/qsci483/math/notation) and [simple linear regression](/courses/qsci483/math/linear-regression/linear-regression) before diving in. 

## The Model ##

Recall that Simple Linear Regression is based upon the equation:
$$
y_i \sim N(X\beta,\sigma^2)
$$
or equivalently 
\begin{align*}
y_i & \sim X\beta + \epsilon_i \\\\
\epsilon_i & \sim N(0,\sigma^2)
\end{align*}
where the $\epsilon_i$ are iid. We have previously derived the estimates for 