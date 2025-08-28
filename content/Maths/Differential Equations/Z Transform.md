---
aliases:
  - z-transform
tags:
  - maths
---

# What is the $\mathcal Z$ Transform?

# Why use the $\mathcal Z$ Transform?

# Explanation

The $\mathcal Z$-Transform plays the same role as the [[Laplace Transform]] in the discrete domain $z$.

$$ x[n]\overbrace{\implies}^{\mathcal Z}\mathcal X(z) $$

The Bilateral $\mathcal Z$-Transform is given by:

$$ \mathcal{X}(z)=\sum_{n=-\infty}^{\infty}x[n]\cdot z^{-n}\\

\text{for }z\in \mathbb{C} $$

The Unilateral $\mathcal Z$-Transform is given by:

$$ \color{green}\mathcal{X}(z)=\sum_{n=0}^{\infty}x[n]\cdot z^{-n}\\

\text{for }z\in \mathbb{C} $$

The $\mathcal Z$-transform of a signal $x[n]$ gives a difference equation as follows:

$$ \mathcal{X}(z)=x[0]z^{-0}+x[1]z^{-1}+...+x[n]z^{-n} $$

All of the values of $z$ make the summation exist in a Region of Convergence (RoC).

## The Complex Variable $z$

$$ z=re^{j\omega}=r\cos(\omega)+j\sin(\omega) $$

where:

- $r$ is the rate of decay or growth.
- $\omega$ is the frequency of oscillation.

## Properties

For a system impulse response $h(n)$ its Z-transform is given by:

$\mathcal H(z)=\sum_{n=-\infty}^{\infty}h(n)\cdot z^{-n}$

Similarly, the Z-transform of a scaled delta function $a\delta(n)$ is given by:

$\mathcal H(z)=\sum_{n=-\infty}^{\infty}a\cdot\delta(n)\cdot z^{-n}=a$

$$ x(n)\overbrace{\iff}^{\mathcal Z}\mathcal X(z) $$

- Linearity: $ax(n)+by(n) \underbrace{\iff}_{\mathcal Z} a\mathcal X(z)+b\mathcal Y(z)$
- Delay: $x(n-n_0) \underbrace{\iff}_{\mathcal Z} z^{-n_0}\mathcal X(z)$
- Convolution: $y(n)=h(n)*x(n) \underbrace{\iff}_{\mathcal Z}\mathcal Y(z)=\mathcal H(z)\cdot \mathcal X(z)$

### Simple Examples of $\mathcal Z$-Transforms

1. Finite-length Sequence
    
    $$ \begin{bmatrix} 1&2&-1&3 \end{bmatrix} \overbrace{\iff}^{\mathcal Z}
    
    1+2z^{-1}-1z^{-2}+3z^{-3} $$
    
2. Impulse
    
    $$ \delta(n)\overbrace{\iff}^{\mathcal Z} \sum_{n=-\infty}^{\infty}\delta(n)z^{-n}=z^{-0}=1 $$
## Stability of a System in the $\mathcal Z$ Domain
![z domain stability](https://www.researchgate.net/publication/271920007/figure/fig2/AS:368804275212289@1464941199326/Stable-and-unstable-regions-for-pole-locations-in-the-z-plane.png)
# Poles and Zeros

$$ G(z)={p_0\prod_{\ell=1}^M(1-\xi_{\ell}z^{-1})

\over

d_0\prod_{\ell=1}^N(1-\lambda_{\ell}z^{-1})} \\ \therefore \\ G(z)=z^{N-M}{ p_0\prod_{\ell=1}^M(z-\xi_{\ell})

\over

d_0\prod_{\ell=1}^N(z-\lambda_{\ell}) } $$

### We define a _pole_ as:

$z=\lambda_\ell \ ; \ G(z)\to \infty$

### We define a _zero_ as:

$z=\xi_\ell \ ; \ G(z)=0$

Note that $G(z)$ has $M$ amount of finite _zeros_ and $N$ amount of finite _poles_.

- $N>M \implies$additional $N-M$ _zeros_ at $z=0$ (origin)
- $N<M \implies$additional $M-N$_poles_ at $z=0$ (origin)

## Example Questions

Given the sequence $x(n)=u(n)$ find the $\mathcal Z$-transform of $x(n)$:

We start with the definition of the $\mathcal Z$-transform

$$ \mathcal X(z)=\sum_{n=0}^{\infty}u(n)z^{-n}=1+z^{-1}+z^{-2}+\dots $$

Using the MacLaurin series expansion we know that

$$ 1+r+r^2+\dots={1\over 1-r} \ where \ |r|\lt1 $$

As such, we implement the same logic to our signal $u(n)$ and say that

$$ \mathcal X(z)={1\over 1-z^{-1}}={z\over z-1} $$

As such we say that when $|z^{-1}|<1\implies|z|>1$

# Regions of Convergence (RoC)

The region of convergence (RoC) is defined as the set of points in the complex plane $\mathbb{C}$ for which the $\mathcal Z$-Transform converges.

Consider the impulse response

$$ \mathcal{X}(z)={1\over 1+0.6z^{-1}} $$

For $\mathcal X(z)$ the region of convergence is given by $|z|>|0.6|$

# BIBO Stability


# Estimating Frequency Response of a Digital System
