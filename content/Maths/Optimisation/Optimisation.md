---
aliases:
  - optimal
  - optimisation
  - Optimization
  - optimization
---
Optimisation looks at minimising defined objective function, which may be constrained or unconstrained. 

---
# Objective Functions

^a835ed

---
# 1. Formulating Optimisation Problems

There are 4 steps involved in the formulation of any optimisation problem. Broadly, these steps involve finding what variables influence the outcome, how those variables function by themselves and all together, defining constraints associated with the problem, and finally solving the problem itself.

## 1. Defining Decision Variables

Decision variables are variables that represent choices or decisions that have to be made to achieve the objective of the problem. Examples of decision variables include quantities of manufacturing outputs (Items A, B, C), and sizes of shapes (length x, y, z). These decision variables are then placed into other variables, normally represented as $x_1, x_2, x_3$.

## 2. Objective Functions

The objective function is usually the part of the question that asks you to optimise (ie maximise or minimise). It is focussed on the output, given the decision variables, and can be thought of as a measure of performance. Some examples include maximise profit or minimise surface area. This is the overall goal of the optimisation.

Objective functions are not always obvious or clear for all optimisation problems. It is possible that you’ll end up in a position where you have to define the objective function in terms of the constraints of the problem.

Once the objective function is found, the [decision variables](https://www.notion.so/Formulating-Optimisation-Problems-ed0fb228f1cc473abfbe4aed8f0e19be?pvs=21) are then adjusted to optimise the objective function.

## 3. Defining Constraints

Constraints are used to limit the optimisation problem to realistic scenarios. This can include physical space limitations, time limitations, resource constraints and the rules of an organisation. The process of defining the constraints has five steps

### I) Identify Constraints

By looking at how the problem is defined, constraints are usually found. Each decision variable should have a form of constraint. Look out for limitations in time, land or people.

### II) Express as Equations

Now that the constraints have been identified, it is time to translate them to maths. For a question with three decision variables, and a constraint of 620, the constraint equation is presented as follows:

$$ 5x_1 + 6x_2 + 6x_3 \le 620 $$

### III) Using the Correct Inequality

Constraints are often presented as inequalities. Using the correct one, and ensuring the inequality symbol is pointing in the correct way is critical. Here are some standard rules for inequalities:

1. if the resource is limited, use the $\le$ symbol.
2. if the resource is infinite, use the $\ge$ symbol.

### IV) Non-negativity Constraint

Each decision variable needs to be greater than zero. Ensure that the inequalities match this.

### V) Check for Consistency

It is important to check that constraints are consistent and do not contradict one another. Hence, constraints should not be mutually exclusive nor contradictory.

## Finalising and Solving the Model

By this point you’ve formulated your optimisation problem and it is ready to be solved. Depending on the complexity of the problem presented, manual methods or computer-based methods may be more appropriate.

### An Example Program

```octave
% Define the decision variables
x = height(3);

% Define the objective function
f = [-52; -56; -48];

% Define the constraints
A = [5 6 6; 4 4 3; 4 4 2];
b = [620; 410; 380];

% Set the lower bound for the decision variables
lb = [0; 0; 0];

% Solve the linear programming problem
[x,fval] = linprog(f,A,b,[],[],lb);

% Print the results
fprintf('Number of Model A desks produced: %d\\n', x(1));
fprintf('Number of Model B desks produced: %d\\n', x(2));
fprintf('Number of Model C desks produced: %d\\n', x(3));
fprintf('Maximum Profit: £%d\\n', -fval);
```