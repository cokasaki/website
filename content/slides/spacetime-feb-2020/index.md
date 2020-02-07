---
authors: [admin]
categories: [spacetime]
date: "2019-02-05T00:00:00Z"
slides:
  highlight_style: dracula
  theme: black
summary: An introduction to using Academic's Slides feature.
tags: []
title: Slides
---

# The SPDE Method for Source Inference

---

## Motivation

- Pollution
	- Say, PCBs
	- Measure PCB concentration in the Duwamish
	- Where did it come from?
	- Easy case: point source
		- Measure all pipe outlets
		- Use regularization method for unknown point source
	- Hard case: non-point source
		- ???

---

## Possible Approaches

- Basically this is the advection-diffusion equation
	- Possibly with linear decay
- Existing methods 
	- fast Fourier transform/Kalman filter (Sigrist et. al 2014)
		- complex method 
		- non-local basis functions
	- finite-difference/finite-element method (Stroud et. al 2010)
		- comparatively simple, very popular
		- local basis functions
		- possibly slower in some contexts
	- Gaussian functional regression (Nguyen and Peraire 2015)
		- also complex, relatively unknown
		- basically a fancy "kernel trick" method

---

## The Math

- Our model is $\mathcal{L}u = f$
- Assume $\mathcal{L}$ is a linear operator
- Then we can discretize this as $Lu = f$
- We model $f\sim X\beta + Z\gamma + \epsilon$
- If everything is normal $[f|d] \sim \mbox{MVN}(\mu_{\rm post},Q_{\rm post}^{-1})$

---

## Pros/Cons

- Fast, lots of sparse matrices
- Flexible modeling on $f$

But:

- Requires known physics
- Requires linear PDE
- Rigid statistical modeling of everything else

--- 

## Demo

- 

---

## Next Steps

- Stationary distribution of a space-time SPDE process
- Stochastic perturbation analysis
- More case studies
	- Animal movement data
	- Snowpack data
	- Pollen data

---


# Questions?
