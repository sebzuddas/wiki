---
tags:
  - maths
---

## Convexity
A function $f : \mathbb{R}^n \rightarrow \mathbb{R}$ is **convex** $\iff \forall \ z_1 \neq z_2$ and  $\forall \ \lambda \in (0, 1)$ the following condition holds:
$$
f(\lambda \cdot z_1 +(1-\lambda)z_2) \leq \lambda f(z_1)+(1-\lambda) \cdot f(z_2)
$$
## Strict Convexity
And the function $f : \mathbb{R}^n \rightarrow \mathbb{R}$ then becomes **strictly convex** $\iff \forall \ z_1 \neq z_2$ and  $\forall \ \lambda \in (0, 1)$ the following condition holds:
$$
f(\lambda \cdot z_1 +(1-\lambda)z_2) \lt \lambda f(z_1)+(1-\lambda) \cdot f(z_2)
$$
The key difference between a function being convex or strictly convex being in the $\le$ and $\lt$ signs. 

![convexity visualised](https://file.notion.so/f/f/ab8f62d9-b9eb-424f-8b5b-bd6a7cc7a150/70c5aae3-777f-466a-b056-1ca39b390659/Convex_vs._Not-convex.jpg?table=block&id=e828df95-6be8-4154-9129-6e45daa5f157&spaceId=ab8f62d9-b9eb-424f-8b5b-bd6a7cc7a150&expirationTimestamp=1756432800000&signature=oYDKzh1vcMHzJVnRtfsQBkBi-tFxoQfX5EHNH8Sg8fg&downloadName=Convex_vs._Not-convex.jpg)


%%
TODO: Implement a graphic, as shown in pg 28/29 of the MPC handbook
TODO: Add Lemma 2.3, where it describes why we need functions to be convex in optimisation
%%



## Proving Convex Functions

Convex function inequality

$$ f(\lambda x_1+(1-\lambda)x_2) \le \lambda f(x_1)+(1-\lambda)f(x_2) $$

1. Pick two points above the function $f(x)$ such as $(x_1, x_2)=(0, 1)$
2. Substitute points into the above inequality. If it holds then the function is convex. If not then the function is not convex.

## Convex, non-differentiable functions

It is possible to have functions that are convex, but not differentiable. One example is the modulus function: $f(x)=|x|$ where $\not \exists f(x)'$.

## Non-convex Functions

# Convex [[Matrices]]

Convex matrices must be [positive semi-definite](https://www.notion.so/Matrices-3858dc1881944e678c5cf01352531937?pvs=21), hence must be [symmetric](https://www.notion.so/Matrices-3858dc1881944e678c5cf01352531937?pvs=21) ie $A=A^T$. The conditions for a PSD matrix are as follows:

### Positive Semi-Definite (PSD)

There are two ways which we can see if a matrix is positive semi-definite.

- 1. Eigen values $\lambda\ge0$
    
    In this case, we need to ensure that all eigenvalues of the matrix are greater than zero. This implies they are all _definitely_ positive, ie $\lambda\ge0\forall \lambda$.
    
- 2. Leading [principle minors](https://www.notion.so/Matrices-3858dc1881944e678c5cf01352531937?pvs=21) $\Delta_{1...n}\ge0$
    
    In this case, we need to ensure that all eigenvalues of the matrix are greater than zero. This implies they are all _definitely_ positive, ie $\Delta\ge0 \forall \Delta$.
    

There is a shortcut to checking whether a matrix is PSD. We check whether the quadratic product is greater than _or equal to_ zero.

$$ \vec x^T \cdot A \cdot \vec x \ge 0 $$

For a given symmetric matrix $A$

$$ A=\begin{pmatrix} 0 & 0\\ 0 & 1 \end{pmatrix} $$

We check for the quadratic product on $A$ giving

$$ \begin{pmatrix} x_1 \\x_2 \end{pmatrix} \cdot \begin{pmatrix} 0 & 0\\ 0 & 1 \end{pmatrix} \cdot \begin{pmatrix} x_1 &x_2 \end{pmatrix} =

(0)^2+x_2^2 \ge 0 $$

Hence, we say $A$ is PSD given that $\vec x$ contains one positive and one zero value.