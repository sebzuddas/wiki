---
aliases:
  - Optimal control
  - optimal control
tags:
  - control
  - engineering
  - modeling
  - maths
  - system
---
# What is Optimal Control?

# Why do we need Optimal Control?

# The Optimal Control Problem
## Mathematical [[System Modelling|Model]]
$$
\vec{\dot{x}} = \vec{f}(t, \vec x, \vec u), \ \ \text{where} \ \ \vec x(t_0)=\vec x_0  \ \ \text{and} \ \ \vec u(t) \in \mathbb U
$$
## Cost [[Functions#^ccba29|Functional]]
$$
J(\vec u) = \int_{t_0}^{t_f}L(t, \vec x(t), \vec u(t))dt + \varphi(t_f, \vec x(t_f))
$$
## Target [[Sets|Set]]
$$
\psi (t_f, \vec x (t_f))=\vec 0
$$
## Problem
Find a input vector $\vec u : [t_0, t_f] \rightarrow \mathbb U$ that guarantees the target set $\psi (t_f, \vec x (t_f))=\vec 0$ whilst minimising $J(\vec u)$.

---
# How?
Optimal control problems are solved through using [[Calculus of Variations]]. 