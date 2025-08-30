---
aliases:
  - Hessian
  - hessian
tags:
  - maths
  - data
---

The Hessian [[Matrices|Matrix]] is a powerful tool used in [[optimisation]] and calculus. It is a square matrix of second-order partial derivatives of a scalar-valued function and serves as a measure of the local curvature of a surface of a higher-dimensional function. **The Hessian Matrix can be used to compute the critical points of a function and to determine the local maxima and minima of the function**. It can also be used to identify saddle points that neither represent maxima nor minima. The Hessian Matrix is a powerful tool that can be used in many applications.

$$H(f) = [H_{ij}(f)]\text{ where }H_{ij}(f) = \frac{\partial^2 f}{\partial x_i \partial x_j}$$

# Properties of the Hessian

## 1. Symmetry - Symmetry of Second Derivatives

The Hessian matrix is always symmetric, meaning that $$
H_{\color{green}{i}\,\color{red}{j}}(f) \;=\; 
H_{\color{red}{j}\,\color{green}{i}}(f) 
\quad \forall \,\color{green}{i}, \color{red}{j}
$$


## 2. Singularity

If the Hessian matrix is **singular**, meaning that it is **not invertible**: it means that the function is not twice differentiable at that point.

# Relationship with the Taylor Series Expansion

The Hessian matrix is common in optimisation. Particularly as part of the Taylor Series Expansion of a function. This example is given below:

### Taylor Series Expansion

$$ y=f(\vec x+\Delta \vec x)\approx f(\vec x)+\nabla f(\vec x)^T \Delta \vec x+{1\over 2} \Delta \vec x^T H(\vec x) \Delta \vec x $$

Where $\Delta \vec x = (x-x_0)$