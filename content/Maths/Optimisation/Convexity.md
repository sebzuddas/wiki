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


%%
TODO: Implement a graphic, as shown in pg 28/29 of the MPC handbook
TODO: Add Lemma 2.3, where it describes why we need functions to be convex in optimisation
%%


