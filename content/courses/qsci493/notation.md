---
date: "2020-03-31T00:00:00+01:00"
draft: false
linktitle: Notation
menu:
  qsci493:
    parent: Math
    weight: 1
title: Notation
toc: true
type: docs
weight: 1
---

## Matrix/Vector Notation ##

In general, I will use an upper case letter for a matrix and a lower case letter for a vector. Vectors such as $v$ will almost always be accompanied by a description of an individual component $v_i$, referring to the $i$-th entry in the vector. If a vector's components are not specified I will bold it such as $\mathbf{v}$. I will write $X'$ to mean the transpose of a matrix $X$ or $v'$ to mean the transpose of a matrix $v$, as is common in regression analysis. 

### Advanced Notation: Matrix Calculus ###

For an $n\times 1$ vector $v$ and a scalar $f$ we will write $\dfrac{\partial v}{\partial f}$ to mean the $n\times 1$ vector with entries $\left(\dfrac{\partial v}{partial f}\right)_i = \dfrac{\partial v_i}{\partial f}$. We will write $\dfrac{\partial f}{\partial v}$ to mean the $n\times 1$ vector with entries $\left(\dfrac{\partial f}{\partial v}\right)_i = \dfrac{\partial f}{\partial v_i}$. A useful resource for matrix calculus can be found on [Wikipedia](https://en.wikipedia.org/wiki/Matrix_calculus) or in the more extensive [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf). 