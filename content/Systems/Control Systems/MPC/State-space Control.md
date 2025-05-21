---
aliases:
  - state space
  - State space
  - State Space
---
# Resources

https://people.isy.liu.se/en/rt/claal20/TSRT09/rteoh3_en.pdf

State-space control relies heavily on [[Matrices]]. 

# Model Form

$\dot{\vec{x}} = A\cdot \vec x + B \cdot \vec u$
$\vec z = C \cdot \vec x + D \cdot \vec u$ ^847543


---
## Reachability
Answers the question: can we move the system to any point in the state space in finite time?

## Controllability
Answers the question: can we stabilise the system around the origin in finite time?

## Stabilisability
Answers the question: can the system be stabilised when closing the loop with a suitably designed $u=Kx$?

## Observability
Answers the question: can we observe the system states without an up-to-date version of $\vec x$?

