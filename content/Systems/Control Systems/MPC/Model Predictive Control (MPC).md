---
aliases:
  - MPC
  - Model-predictive Control
  - model-predictive control
  - model predictive control
---
# What is MPC?

**Model Predictive Control** is a control strategy that uses a **dynamic model of the system** to **predict future behaviour** and **optimise control inputs** over a finite time horizon. It solves an **optimisation problem** at each control step to determine the best action, while explicitly handling **constraints** on inputs, states, and outputs.

# Why use MPC?
In control engineering, we’re often faced with the task of designing a controller that not only keeps the system stable but also ensures good performance under **physical and operational constraints**. Classical control strategies—like PID, pole placement, or even [[State-space Control|LQR]]—tend to struggle when the system becomes complex, multivariable, nonlinear, or subject to tight limits on inputs or states. That’s where **Model Predictive Control** comes in.

MPC stands out by making **explicit use of a model of the system** to anticipate how the system will evolve over a future time horizon. Instead of reacting to the current error only (as in PID), MPC **optimises a sequence of control inputs** to achieve the best future behaviour, while ensuring that no physical or safety limits are violated. This **predictive and optimising nature** is what makes MPC uniquely powerful for modern, constraint-laden control problems.

# Overview
Model Predictive Control, MPC, is an optimal control technique. 

Following a [[State-space Control|state space]] model of the form:
![[State-space Control#^847543]]


The optimal control problem is formulated as follows:

$$
\begin{align*}
\min_{\{u_k, \dots, u_{k+N-1}\}} \quad & \sum_{i=0}^{N-1} \left( \|x_{k+i} - x_{\text{ref}}\|_Q^2 + \|u_{k+i} - u_{\text{ref}}\|_R^2 \right) + \|x_{k+N} - x_{\text{ref}}\|_P^2 \\
\text{subject to} \quad & x_{k+i+1} = A x_{k+i} + B u_{k+i}, \quad i = 0, \dots, N-1 \\
& x_{k+i} \in \mathcal{X}, \quad u_{k+i} \in \mathcal{U}, \quad i = 0, \dots, N-1 \\
& x_{k+N} \in \mathcal{X}_f \\
& x_k \text{ given}
\end{align*}

$$
where
- $N$: Prediction horizon
- $x_k$​: Current state
- $x_{\text{ref}}, u_{\text{ref}}$​: Reference state and input
- $Q,R,P$: Positive semi-definite weighting matrices
- $\mathcal{X}, \mathcal{U}$: State and input constraint sets
- $\mathcal{X}_f$​: Terminal constraint set (optional, used for stability guarantees)

## Dual-mode MPC
Dual-mode MPC is a control strategy that splits the future control horizon into two modes:

1. **Mode 1 (MPC mode)**: The controller optimises the input sequence over a finite horizon as usual.
2. **Mode 2 (Terminal mode)**: After the finite horizon, it assumes a **known stabilising controller** (e.g., LQR) will take over and drive the system to the origin (or reference point).

This structure gives formal guarantees of **closed-loop stability**, because the optimisation ensures that the system ends up in a **terminal region** where the stabilising controller can handle everything safely from there.
$$
\begin{align*}
\min_{\{u_k, \dots, u_{k+N-1}\}} \quad & \sum_{i=0}^{N-1} \left( \|x_{k+i} - x_{\text{ref}}\|_Q^2 + \|u_{k+i} - u_{\text{ref}}\|_R^2 \right) + \|x_{k+N} - x_{\text{ref}}\|_P^2 \\
\text{subject to} \quad & x_{k+i+1} = A x_{k+i} + B u_{k+i}, \quad i = 0, \dots, N-1 \\
& x_{k+i} \in \mathcal{X}, \quad u_{k+i} \in \mathcal{U}, \quad i = 0, \dots, N-1 \\
& x_{k+N} \in \mathcal{X}_f \\
& x_k \text{ given}
\end{align*}
$$

### Key Additions with Dual-mode MPC
- **Terminal Cost** $\|x_{k+N} - x_{\text{ref}}\|_P^2$ ​
    - $P$ is chosen as the **solution to the discrete-time algebraic Riccati equation (DARE)** for the stabilising feedback controller $u=Kx$.
    - This approximates the infinite-horizon cost-to-go if you were to use $u=Kx$ after the horizon ends.
- **Terminal Constraint Set** $\mathcal{X}_f$​
    - This is a **positive invariant set** under the stabilising control law $u=Kx$.
    - It ensures that if $x_{k+N} \in \mathcal{X}_f$​, then future states will remain within constraints even after switching to the backup controller.
- **Backup Controller $u=Kx$** (used implicitly)
    - Not part of the optimisation, but used in the **theoretical analysis**.
    - Ensures that once the system reaches $\mathcal{X}_f$​, it can be safely stabilized indefinitely.



[[Moving Horizon Estimation (MHE)]]


