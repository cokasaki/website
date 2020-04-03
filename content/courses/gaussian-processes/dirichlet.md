---
date: "2020-04-01T00:00:00+01:00"
draft: false
linktitle: Dirichlet BCs
menu:
  gaussian-processes:
    parent: SPDEs
    weight: 5
title: Dirichlet BCs
toc: true
type: docs
weight: 5
---

The simplest version of the finite element method is said to have "natural" Neumann boundary conditions, meaning that Neumann boundary conditions are naturally satisfied without imposing any additional structure. Dirichlet boundary conditions then are said to be "essential," meaning they must be explicitly imposed after the fact. Certain FEM formulations change up this Neumann=natural, Dirichlet=essential pradigm but for our purposes we will treat these terms as interchangeable.

Now, suppose that we are confronted with the FEM equation $$\begin{bmatrix} K_{11} & K_{12} \\\\ K_{21} & K_{22} \end{bmatrix}\begin{bmatrix} u_1 \\\\ u_2\end{bmatrix} = \begin{bmatrix} L_{11} & L_{12} \\\\ L_{21} & L_{22} \end{bmatrix} \begin{bmatrix} f_1 \\\\ f_2 \end{bmatrix},$$
with the Dirichlet BC $u_2 = u^*$. We can modify our FEM equation to enforce this, to $$\begin{bmatrix} K_{11} & K_{12} \\\\ 0 & I \end{bmatrix}\begin{bmatrix} u_1 \\\\ u_2\end{bmatrix} = \begin{bmatrix} L_{11}f_1 + L_{12}f_2 \\\\ u^* \end{bmatrix}.$$ We can simplify this to an equation for $u$ by inverting the matrix on the left: $$\begin{bmatrix} u_1 \\\\ u_2 \end{bmatrix} = \begin{bmatrix} K_{11}^{-1} & -K_{11}^{-1}K_{12} \\\\ 0 & I \end{bmatrix} \begin{bmatrix} L_{11}f_1 + L_{12}f_2 \\\\ u^* \end{bmatrix}$$
Now, assuming that $f\sim N(\mu_f,Q_f)$ and $u^*$ is given (often $u^* = 0$) we obtain the following distribution for $u_1$:
\begin{align*}
u_1 & = K_{11}^{-1}L_1f - K_{11}^{-1}K_{12}u^* \\\\
u_1 & \sim N\left(K_{11}^{-1}(L_1\mu_f - K_{12}u^*), K_{11}^{-1}L_1\Sigma_fL_1^TK_{11}^{-T}\right).
\end{align*}

Now often we are interested in the posterior distribution for $u$ and/or $f$. Since we have assumed that $u^\*$ is given the two are deterministically related. However, $u$ is of lower dimension than $f$. So, we will try to calculate the posterior distribution of $f$ since this can be used to calculate the posterior of $u$ but not vice versa. We will assume that we have made some observations $y = y\_1 + y\_2 = A\_1u\_1 + A\_2u\_2 + \epsilon$ of $u$ from which we wish to make inference. Since $u^\*$ is known we essentially have observations:
\begin{align*}
y & = A_1K_{11}^{-1}(L_1f - K_{12}u^\*) + A_2u^\* + \epsilon \\\\
& = A_1K_{11}^{-1}L_1f + (A_2 - A_1K_{11}^{-1}K_{12})u^\* + \epsilon \\\\
& = y_f + y^\* + \epsilon
\end{align*}
Using the [results](../posteriors/#linear-observations) for a posterior from a linearly-observed MVN distribution we see that:
\begin{align*}
[f|y=a] & = N\left(\mu_f + Q_{f|y}^{-1}B^TQ_{\epsilon}(y - y^* - B^T\mu_f), Q_{f|y}^{-1}\right) \\\\
Q_{f|y} & = Q_f + B^TQ_{\epsilon}B \\\\
B & = A_1K_{11}^{-1}L_1
\end{align*}

So the challenges that we face in implementing these two different ideas are as follows:
- $u_1$ is lower dimensional than $f$ and so $f$ is not uniquely defined by $u_1$
- $L_1\Sigma_fL_1^T$ is not easily invertible since $L_1$ is not square
- OR: $K_{11}^{-1}$ is dense

My solution to this is to take the approach of modeling at the $u$ level. We can deal with $u_1$ being lower dimensional by calculating the degenerate MVN distribution for $f|u_1$. We can deal with $L_1$ being rectangular by considering our old approximation $\tilde{L}$. Using this approximation, which we have always needed to do to keep $L^{-1}$ sparse, we have that $(\tilde{L}\_1\Sigma_f\tilde{L}\_1^T)\_{ij} = \tilde{L}\_{ii}\Sigma\_{ij(f)}\tilde{L}\_{jj}$ for $1\leq i,j\leq n_1$. This is exactly the same as if we truncated the additional rows and columns of $\tilde{L}$ and $\Sigma_f$, since $\tilde{L}$ is zero in all of those columns anyways. Thus we obtain
\begin{align*}
(L_1\Sigma_fL_1^T)^{-1}
& \approx (\tilde{L}_1\Sigma_f\tilde{L}_1^T)^{-1} \\\\
& = (\tilde{L}_{11}\Sigma_{11(f)}\tilde{L}_{11})^{-1} \\\\
& = \tilde{L}^{-1}_{11}\Sigma_{11(f)}^{-1}\tilde{L}_{11}^{-1} \\\\
& = \tilde{L}^{-1}_{11}(Q_{11(f)} - Q_{12(f)}Q_{22(f)}^{-1}Q_{21(f)})\tilde{L}^{-1}_{11}.
\end{align*}
Since $u_2$ is low dimensional, we can easily invert the dense $Q_{22}$ and we still end up with a sparse matrix $Q_{11(u)}$. So we have solved problem (1).

Now we hit up against the following problem: we may conduct efficient inference on $u_1$, but even though $u_1$ and $f$ are deterministically linked, $f$ is not uniquely identified by $u_1$. So we need to find the degenerate MVN distribution defining $f|u_1$. Alternatively we can assume that there is some small noise that enters between $f$ and $u_1$ so that the two are _not_ deterministically linked and so that we have a non-degenerate distribution for $f$. This becomes more important if we have measurements for $f$ as well as $u$. For now we will assume this is not the case.

The primary obstacle we need to overcome here is how to obtain inference for a degenerate distribution while maintaining our sparsity paradigm, which depends upon the inverse of the covariance matrix. To obtain the distribution for $f|u_1$ we will consider the following algebra:
\begin{align*}
[f|u_1]
& = [f_1,f_2|u] \\\\
& = [f_1|u,f_2][f_2|u].
\end{align*}
So what we are able to do here is specify each of these distributions separately. Since all we really need to be able to do is calculate the posterior mean of $f$ and simulate draws of $f|u$, if we can do this we are finished. So what are these distributions? Once $f_2$ is specified, $f_1$ is uniquely determined by $u_1$ so we find that: $$[f_1|u_1,f_2] = L_{11}^{-1}(K_{11}u_1 + K_{12}u^* - L_{12}f_2).$$ So then all we really need to sample is $[f_2|u]$. This can be calculated using Bayes formula:
\begin{align*}
[f_2|u]
& = [u|f_2][f_2] \\\\
& = [u_1|f_2,u^*][f_2]
\end{align*}
Then we can use the formula $$K_{11}u_1 + K_{12}u^* = L_{11}f_1 + L_{12}f_{2}$$
to obtain $$u_1 = K_{11}^{-1}(L_{11}f_1 + L_{12}f_2 - K_{12}u^*)$$
and the (non-degenerate MVN) distribution
\begin{align*}
u_1
& = N\left(K_{11}^{-1}(L_{11}\mu_{1(f)} + L_{12}f_2 - K_{12}u^*),K_{11}^{-1}L_{11}\Sigma_{11(f)}L_{11}K_{11}^{-T}\right) \\\\
& \approx N\left(K_{11}^{-1}(L_{11}\mu_{1(f)} + L_{12}f_2 - K_{12}u^*),K_{11}^{T}\bar{L}_{11}^{-1}\Sigma_{11(f)}^{-1}\bar{L}_{11}^{-1}K_{11}\right) \\\\
& = N\left(K_{11}^{-1}(L_{11}\mu_{1(f)} + L_{12}f_2 - K_{12}u^*),K_{11}^{T}\bar{L}_{11}^{-1}(Q_{11(f)} - Q_{12(f)}Q_{22(f)}^{-1}Q_{21(f)})\bar{L}_{11}^{-1}K_{11}\right)
\end{align*}
Now this distribution bears a remarkable resemblance to what we have previously written for $[u_1]$ with no conditioning at all, since our approximation $\tilde{L}$ removes the direct dependence of $u_1$ on $f_2$. However, it is important to note that in this formulation we _actually_ have the matrix $L_{11}$, rather than a truncation of $\tilde{L}$. Thus we need to approximate $L_{11}$ directly rather than approximation $\tilde{L}\approx L$. Thus we get what we have denote $\bar{L}_{11}\approx L_{11}$ which we obtain by grouping all the terms in $L_{11}$ onto the diagonal (the same operation which gives us $\tilde{L}$ from $L$). Since a number of non-zero terms from $L$ are removed by this operation, and in particular these non-zero terms are along the edges of $u_1$ where it abuts $u_2$, we see that in fact this is a substantively different distribution. Our approximation for $[u_1|f_2]$ has less uncertainty along the boundary, just as we would expect. Now that we have this distribution all we need do is find the unconditional distribution $[f_2]$, which may easily be found by blockwise inversion of $Q_{f}$: $$[f_2] = N(\mu_{2(f)},(Q_{22} - Q_{21}Q_{11}^{-1}Q_{12})^{-1}).$$ This involves a dense $n_2\times n_2$ matrix but since the boundary is lower-dimensional this is okay.

Now actually calculating this full posterior involves some _heavy_ algebra and I'm sure there's a better way to do this but here goes:
\begin{align*}
[f_2|u_1]
& \propto [u_1|f_2][f_2] \\\\
& = N\left(u_1|K_{11}^{-1}(L_{11}\mu_1 + L_{12}f_2 - K_{12}u^*),K_{11}^{-1}\bar{L}_{11}\Sigma_{11}\bar{L}_{11}K_{11}^{-T}\right)N\left(f_2|\mu_2,\Sigma_{22}\right) \\\\
& \propto \exp\left(-\frac{1}{2}(u_1 - \mu_{u|f})^TK_{11}^T\bar{L}_{11}^{-1}\Sigma_{11}^{-1}\bar{L}_{11}^{-1}K_{11}(u_1 - \mu_{u|f})\right)\exp\left(-\frac{1}{2}(f_2-\mu_2)^T\Sigma_{22}^{-1}(f_2-\mu_2)\right) \\\\
& \propto \exp\left(-\frac{1}{2}f^T(\Sigma_{22}^{-1}+L_{12}^T\bar{L}_{11}^{-1}\Sigma_{11}^{-1}\bar{L}_{11}^{-1}L_{12})f - \frac{1}{2}(f^Tv + v^Tf)\right) \\\\
v & = L_{12}^T\bar{L}_{11}^{-1}\Sigma_{11}^{-1}\bar{L}_{11}^{-1}(L_{11}\mu_1 + L_{12}f_2 - K_{12}u^* - K_{11}u_1) - f_2^T\Sigma_{22}^{-1}\mu_2
\end{align*}
Thus we see that $[f_2|u_1]$ is normal with precision matrix:
\begin{align*}
Q_{f_2|u_1}
& = \Sigma_{22}^{-1} + L_{21}\bar{L}_{11}^{-1}\Sigma_{11}^{-1}\bar{L}_{11}^{-1}L_{12} \\\\
& = Q_{22} - Q_{21}Q_{11}^{-1}Q_{12} + L_{21}\bar{L}_{11}^{-1}(Q_{11} - Q_{12}Q_{22}^{-1}Q_{21})\bar{L}_{11}^{-1}L_{12} \\\\
\mu_{f_2|u_1}
& = -Q_{f_2|u_1}^{-1}v
\end{align*}