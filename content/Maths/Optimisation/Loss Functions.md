---
tags:
  - maths
---

Loss functions are a subset of [[Optimisation#^a835ed|Objective functions]]


There are a number of ways to minimise functions. We start by looking at how to minimise a bivariate function analytically.

# Minimise $f(x)$ analytically:

$$ f(x_1, x_2) = 5x_1^2+x_2^2-2x_1x_2-14x_1 +2x_2+19 $$

### Step 1: 1st order conditions

Firstly, we define what it means for a point to be optimal. We know that an optimal point of the function $f(x)$ will have zero gradient. Hence, we check for the condition: $\nabla f(x)=0$.

To check for this condition, we perform partial differentiation as follows:

${\partial\over \partial x_1}= 10x_1-2x_2-14 = 0$

${\partial\over \partial x_2}= 2x_2-2x_1+2 = 0$

Through using simultaneous equations, we find the values of $(x_1, x_2) = [{3\over 2}, {1\over2}]$.

- Hint:
    
    When finding the values through simultaneous equations, don’t substitute the values found.
    

### Step 2: 2nd order conditions

We now look for second order conditions, implying we need to find the values inside the Hessian matrix of the function:
$$\nabla^2 f(x)=\begin{pmatrix} {\partial^2\over \partial x_1^2}&{\partial^2\over \partial x_1x_2} \\ {\partial^2\over \partial x_1x_2}&{\partial^2\over \partial x_2^2} \end{pmatrix}$$

- Hint:
    
    Remember ${\partial^2\over \partial x_1x_2} = {\partial\over \partial x_1}({\partial\over \partial x_2})= {\partial\over \partial x_2}({\partial\over \partial x_1})$
    
    As such we get $\nabla^2f(x)=\begin{pmatrix} 10&-2 \\ -2&2 \end{pmatrix}$
    

Now that we’ve formed the Hessian matrix, 

$$\nabla^2 f(x)=

\begin{pmatrix} 10&-2 \\ -2&2 \end{pmatrix}$$ 

we check to see whether the first two principle minors, 

$$\Delta_1, \Delta_2 >0$$

- Hint:
    
    Calculating minors of a given matrix $A$, the first minor $\Delta_1$ is the _determinant_ of all rows and columns _including and before $a_{1,1}$._ By extension, $\Delta_2$ is the _determinant_ of all rows and columns _including and before $a_{2,2}$._
    

As such, we get:

$\Delta_1=10$

$\Delta_2=\det\begin{bmatrix} 10&-2 \\ -2&2 \end{bmatrix}=16$

We then can confirm that $\Delta_1, \Delta_2 >0$ hence both 1st and 2nd order conditions are satisfied and we can declare $x^*=(x_1, x_2) = [{3\over 2}, {1\over2}]$ as our optimal point.

# Perform one iteration of the gradient method on $f(x)$:

$$ f(x_1, x_2) = x_1^2-x_1x_2+2x_2^2+x_1-x_2+1 $$

Then find the following properties:

1. The moving direction at the initial point $d^{(0)}$
2. The value of the step size at the initial point $\alpha^{(0)}$ for $x^{(1)}=x^{(0)}+\alpha d^{(0)}$

## Moving Direction

$$d^{(0)}= \begin{bmatrix} -1 \\ 1 \end{bmatrix}$$

- Hint:
    
    The moving direction is given by the following definition: $\vec d = -\nabla f(x)$
    
    Work out the gradient $\nabla f(x)$ first.
    

## Step Size

Put $\alpha$ into a function $g(\alpha)$, minimise the function such that 
$$
a^{(0)} = \min_\alpha g(\alpha) = \tfrac{1}{8}
$$

- Hint:

$$
x^{(1)} = \begin{bmatrix} x_1^{(1)} \\ x_2^{(1)} \end{bmatrix}
= \begin{bmatrix} 0 \\ 0 \end{bmatrix}
+ \alpha \begin{bmatrix} -1 \\ 1 \end{bmatrix}
= \begin{bmatrix} -\alpha \\ \alpha \end{bmatrix}
$$

We then create a function $g$ wrt $\alpha$, giving

$$
g(\alpha) = f(x^{(0)}+\alpha d^{(0)}) = f(x^{(1)}) 
= f(x_1^{(1)}, x_2^{(1)}) = f(-\alpha, \alpha) 
= 4\alpha^2 - 2\alpha + 1
$$
