---
date: "2020-04-01T00:00:00+01:00"
draft: false
linktitle: Block Matrices
menu:
  gaussian-processes:
    parent: Properties
    weight: 4
title: Block Matrices
toc: true
type: docs
weight: 4
---

Block matrices have some nice linear algebraic properties:
- $$\begin{bmatrix}
A & B \\\\
C & D
\end{bmatrix}
=
\begin{bmatrix}
A^{-1} + A^{-1}B(D-CA^{-1}B)^{-1}CA^{-1} & -A^{-1}B(D-CA^{-1}B)^{-1} \\\\
-(D-CA^{-1}B)^{-1}CA^{-1} & (D-CA^{-1}B)^{-1}
\end{bmatrix}$$
- $$\begin{bmatrix}
A & B \\\\
C & D
\end{bmatrix}
=
\begin{bmatrix}
(A-BD^{-1}C)^{-1} & -(A-BD^{-1}C)^{-1}BD^{-1} \\\\
-D^{-1}C(A-BD^{-1}C)^{-1} & D^{-1} + D^{-1}C(A-BD^{-1}C)^{-1}BD^{-1}
\end{bmatrix}$$
- $$\mbox{det}
\begin{pmatrix}
A & B \\\\
C & D
\end{pmatrix}
=
\mbox{det}(A)\times \mbox{det}(D-CA^{-1}B) = \mbox{det}(D)\times\mbox{det}(A-BD^{-1}C)$$