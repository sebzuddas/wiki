---
aliases:
  - SINDy
---

# What is SINDy?
Sparse Identification of Nonlinear Dynamical Systems (SINDy) is a relatively new [(2016)](https://arxiv.org/abs/1509.03580) [[System Identification]] technique. It uses an approach called Sparse Regression to find optimal parameters, a key difference to traditional system identification techniques which use linear regression techniques.

# Why use SINDy?

## 1. Convenience
Although SINDy is only a method for conducting [[System Identification|system identification]], it is different enough from traditional techniques to warrant its own page. Whilst [[System Identification#^088a7d|NARMAX]] is an appropriate technique for nonlinear system identification, it has a key limitation. [[System Identification#^088a7d|NARMAX]] outputs a linear equation in the time domain, whereas SINDy outputs a [[State-space Control|state space]] equation, making it particularly convenient to develop models such using advanced control techniques such as [[Model Predictive Control (MPC)]].

## 2. Low Data Resolution
Another significant advantage of SINDy is that it can correctly identify system dynamics even with a small dataset, a significant advantage when compared to NN-based identification techniques. 

## 3. Enforcing Physics
SINDy has the capability to pre-embed physical 

## 4. Handling Nonlinearity
Autoregressive models such as ARX, ARMAX etc. assume that the modelled system is *linear*. 

# SINDy for Control (SINDYc)

The SINDy framework was extended in 2018 to encompass control. The paper [SINDy for Control](https://royalsocietypublishing.org/doi/10.1098/rspa.2018.0335) demonstrates the potential of SINDy to enable MPC in non-conventional fields. 

## The SINDYc Framework

![image](https://royalsocietypublishing.org/cms/asset/7386f85e-7158-41f0-af80-26c90b3198a8/rspa20180335f02.jpg)

