---
aliases:
  - ODE
  - ODEs
  - Differential Equations
  - differential equations
  - Differential equations
  - differential equation
  - Differential equation
---

# What are Differential Equations?
ODEs are essential in mathematics, physics, engineering, and other fields because they describe the behaviour of dynamic systems. When something changes over time or space (like velocity, temperature, or population), ODEs help us model those changes.


# First Order ODE's
## First-Order Ordinary Differential Equation:

A first-order ODE can be written in the general form:

$$
\frac{dy}{dx} = f(x, y)
$$

Where:
- $y = y(x)$ is the unknown function of $x$,
- $f(x, y)$ is a given function of $x$ and $y$.

### Example:

$$
\frac{dy}{dx} = {-2y}
$$

The solution can be written as:

$$
y(x) = C e^{{-2x}}
$$

Where $C$ is the constant of integration.

---

# Second-Order ODE 
## Second-Order Ordinary Differential Equation:

A second-order ODE has the form:

$$
\frac{d^2y}{dx^2} = g(x, y, \frac{dy}{dx})
$$

Where:
- $y = y(x)$ is the unknown function of $x$,
- $g(x, y, \frac{dy}{dx})$ is a given function of $x$, $y$, and $\frac{dy}{dx}$.

### Example:

$$
\frac{d^2y}{dx^2} = {-3y}
$$

The solution is:

$$
y(x) = C_1 \cos(\sqrt{{3}}x) + C_2 \sin(\sqrt{{3}}x)
$$

Where $C_1$ and $C_2$ are constants of integration.

---

# System of ODEs 
## System of First-Order ODEs:

A system of first-order ODEs can be written as:

$$
\frac{d\mathbf{y}}{dx} = \mathbf{F}(x, \mathbf{y})
$$

Where:
- $\mathbf{y} = \begin{pmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{pmatrix}$ is a vector of unknown functions,
- $\mathbf{F}(x, \mathbf{y}) = \begin{pmatrix} f_1(x, \mathbf{y}) \\ f_2(x, \mathbf{y}) \\ \vdots \\ f_n(x, \mathbf{y}) \end{pmatrix}$ is a vector of functions.

### Example:

$$
\frac{d}{dx} \begin{pmatrix} y_1 \\ y_2 \end{pmatrix} = \begin{pmatrix}{y_2} \\ {-y_1} \end{pmatrix}
$$

This system describes a coupled set of first-order ODEs:

$$
\frac{dy_1}{dx} = {y_2}, \quad \frac{dy_2}{dx} = {-y_1}
$$

The solution to this system is:

$$
y_1(x) = C_1 \cos(x) + C_2 \sin(x), \quad y_2(x) = -C_1 \sin(x) + C_2 \cos(x)
$$

Where $C_1$ and $C_2$ are constants of integration.
