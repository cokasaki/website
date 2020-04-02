---
date: "2020-04-01T00:00:00+01:00"
draft: false
linktitle: Linear Algebra
menu:
  gaussian-processes:
    parent: Properties
    weight: 3
title: General Linear Algebraic Properties
toc: true
type: docs
weight: 3
---

The Woodbury matrix identity is $$(A+UCV)^{-1}=A^{-1} - A^{-1}U(C^{-1}+VA^{-1}U)^{-1}VA^{-1}.$$ Simpler versions of this identity are
\begin{align*}
(I+UV)^{-1} & = I-U(I+VU)^{-1}V, \\\\
(I+P)^{-1} & = I-(I+P)^{-1}P \\\\
(I+P)^{-1} & = I-P(I+P)^{-1}
\end{align*}
In the special case that $u$ and $v$ are vectors and $C = I$ we get the Sherman-Morrison formula: $$(A+uv^T)^{-1} = A^{-1} - \frac{A^{-1}uv^TA^{-1}}{1+v^TA^{-1}u}.$$ A similar formula is the matrix determinant lemma $$\mbox{det}(A+uv^T) = (1+v^TA^{-1}u)\mbox{det}(A).$$