---
date: "2020-04-02T00:00:00+01:00"
draft: false
linktitle: Pseudoinverse
menu:
  gaussian-processes:
    parent: Properties
    weight: 6
title: Moore-Penrose Pseudoinverse
toc: true
type: docs
weight: 6
---

The Moore-Penrose Pseudoinverse is a generalization of the inverse. In particular it extends the inverse matrix to non-square matrices. The inverse of a matrix $A$ is defined by any matrix $A^+$ with the following four properties
- $AA^+A = A$
- $A^+AA^+ = A^+$
- $(AA^+)^* = AA^+$
- $(A^+A)^* = A^+A$

If $A$ has linearly independent columns then the pseudoinverse can be calculated as $A^+ = (A^*A)^{-1}A^*$ and the pseudoinverse is a _left inverse_ since $A^+A = I$. If $A$ has linearly independent columns then the pseudoinverse can be calculated as $A^+ = A^*(AA^*)^{-1}$ and the pseudoinverse is a _right inverse_ since $AA^+ = I$.

Finally, consider $(AB)^+$. This is equal to $B^+A^+$ if:
- $A$ has orthonormal columns (i.e. $A^*A = I$), or
- $B$ has orthonormal rows (i.e. $BB^* = I$), or
- $A$ has full column rank and $B$ has full row rank, or
- $B = A^*$
In general, however, $(AB)^+ \neq $B^+A^+$. 
