---
title: Control Systems Review
date: 2023-04-24 11:05:34
categories: Review
tags:
  - college
  - control systems
  - review
description: Review notes of Control Systems.
---


<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> </small>

---

## Concepts

### Terminology

**$S_\theta$**: system to be controlled

**$C$**: controller

**$\theta$**: system's parameters

<mark style="background-color: #9fc5e8;">**$y$**: controlled variable (output)</mark>

<mark style="background-color: #9fc5e8;">**$u$**: control variable (accessible to the controller)</mark>

**$d$**: disturbance

**$\omega$**: reference variable (set-point)

**$\bar d, \bar \theta$**: known nominal values of $d, \theta$

$ d = \bar d + \Delta d $

$ \theta = \bar \theta + \Delta \theta $

**$e$**: error variable

$ e(t) = \omega (t) - y(t) $

<mark style="background-color: #9fc5e8;">**$x$**: state variable</mark>


### Open-loop stratergy 

### Closed-loop stratergy


### Basic requirements

$$ y(t) ≃ \omega (t) $$

$ e(t) ≃ 0 $ in all situations of interest. 




---

## Fundamentals of Systems theory

### Dynamic systems

*Can $y(t)$ be determined only through the knowledge of $u(t)$?*

If the answer is *no*, then the system is a dynamic one. 


### Definitions

**Strictly proper dynamic system**: the output function depends only on the state, not depend on the input. 

**Time-invariant (or stationary)**: both input and output do not explicitly depend on time. 

**Linear**: input and output functions depend linearly on $x, u$. 

**Single input single output**: SISO (orders of input and output)

**Multi-input multi-output**: MIMO 

### <mark style="background-color: #9fc5e8;">State equations</mark>

Non-dynamic systems have no need to introduce state variables. 

$$input: \quad u(t) = [ u_1(t), \ ... \ , u_m(t) ]^T $$

$$state: \quad x(t) = [ x_1(t), \ ... \ , x_n(t) ]^T $$

$$output: \quad y(t) = [ y_1(t), \ ... \ , y_p(t) ]^T $$

$$ \dot x(t) = f( x(t), u(t), t ) $$
$$ y(t) = g( x(t), u(t), t ) $$

#### Determination of <mark style="background-color: #9fc5e8;">state and output trajectories</mark>

*Given $t_0, x(t_0), u(t)$, find $x(t)$ (state trajectory) and $y(t)$ (output trajectory).*

Step 1: Find the solution of $ f(x, u, t) $. (Integration or Laplace transform)

Step 2: Plug the solution into $ g(x, u, t) $. (Substitution)


For time-invariant systems we set $t_0 = 0$ without loss of generality, because there is no dependency on time in f and g. 

### LTI MIMO dynamic systems

$$ \dot x(t) = Ax + Bu $$
$$ y(t) = Cx + Du $$

$$ x \in \mathbb{R}^n \quad u  \in \mathbb{R}^m \quad y  \in \mathbb{R}^p $$
$$ x(0) = x_0 \quad u(t), \ t \geq 0 $$

$$x(t) = e^{At}x(0) + \int_{0}^{t}e^{A(t-\tau)}Bu(\tau)d\tau$$

where the matrix exponential is defined as:

$$e^{At} = \sum_{k=0}^{\infty}\frac{A^kt^k}{k!}$$

The first term depends linearly only the initial state, while the second term depends only the input. 

Let $ x(t) = x_l(t) + x_f(t) $

which are the **free state trajectory** and the **forced state trajectory**. 

### Equilibrium state

Constant trajectory of $x(t)$ for a constant input $u(t) = \bar u, \forall t$. 

All equilibrium states can be determined by selecting inputs $ u(t) = \bar u \in \mathbb{R}^m $. 

<mark style="background-color: #9fc5e8;">**Equilibrium states $\bar x$**</mark> are the solutions of the algebraic equation $f(x, \bar u)=0$

<mark style="background-color: #9fc5e8;">**Equilibrium outputs**</mark> are obtained by substituting $\bar x, \bar u$ into the output equation $ \bar y = g(\bar x, \bar u) $. 

If $det(A) \not = 0$, $ \bar x = -A^{-1} B \bar u $. (one and only one equilibrium state)

Then $\bar y = (-CA^{-1}B+D) \bar u $, where $(-CA^{-1}B+D)$ is called the <mark style="background-color: #9fc5e8;">**static gain**</mark>. 

This only exists if matrix A is invertible, so when $det(A)=0$, we have infinite $\bar x, \bar y$ or no $\bar x, \bar y$. 

### Linearisation

The basic idea is to do the derivations of the state equations and use the derivative as a linear approximation. 




---

## Stability

### Linear systems

### Properties of asymptotically stable linear systems



### Analysis of $e^{At}$

#### A is diagonal


#### A has real and distinct eigenvalues

#### A has complex and distinct eigenvalues


#### A has eigenvalues with multiplicity larger than 1



### Criteria based on analysis elements of matrix A


### <mark style="background-color: #9fc5e8;">Routh table</mark>


---



