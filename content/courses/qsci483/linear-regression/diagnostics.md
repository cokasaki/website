---
date: "2020-04-01T00:00:00+01:00"
draft: false
linktitle: Using Diagnostics
menu:
  qsci483:
    parent: Linear Regression
    weight: 5
title: Using Model Diagnostics
toc: true
type: docs
weight: 5
---

In this document I will outline the math used to derive and analyze the model diagnostics for a simple linear regression model. Make sure to check out my previous posts on [notation](/courses/qsci483/notation) and [linear regression](/courses/qsci483/linear-regression/linear-regression) before diving in. 

Plotting fitted vs residuals
- should see no trend/pattern
- centered on zero

Plot histogram of residuals
- basically centered on zero?
- skewed/kurtosis?
- Shapiro-Wilks test (can be very sensitive to small deviations with large datasets)

QQ plot

DFFITS
Deleted residuals
Cook's distance and leverage