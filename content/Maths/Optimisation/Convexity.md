A function $f : \mathbb{R}^n \rightarrow \mathbb{R}$ is **convex** $\iff \forall \ z_1 \neq z_2$ and  $\forall \ \lambda \in (0, 1)$ the following condition holds:
$$
f(\lambda \cdot z_1 +(1-\lambda)z_2) \leq \lambda f(z_1)+(1-\lambda) \cdot f(z_2)
$$
And the function $f : \mathbb{R}^n \rightarrow \mathbb{R}$ then becomes **strictly convex** $\iff \forall \ z_1 \neq z_2$ and  $\forall \ \lambda \in (0, 1)$ the following condition holds:
$$
f(\lambda \cdot z_1 +(1-\lambda)z_2) \lt \lambda f(z_1)+(1-\lambda) \cdot f(z_2)
$$
The key difference between a function being convex or strictly convex being in the $\le$ and $\lt$ signs. 