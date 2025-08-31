---
aliases:
  - functions
  - function
  - Function
tags:
  - maths
---
# What is a Function?
## Basic Definition
A function is a mathematical operation that describes how a variable is altered, based on the inner workings of the function. Functions themselves can be thought of as processes, and by inputting a value to the process, we get the result of what the process does to the value. 

$$
x\rightarrow \fbox{function()} \rightarrow f(x)
$$
Above, the input $x$ is passed into the function $function()$. The result of inputting $x$ into the function is the output $f(x)$. 
### Visualisation
When visualising the function $f(x) = x$ we see a line. This line tells us that for every time we input 1, we get 1 as an output. As we go along the number line on the horizontal axis, we go along the number line at the same rate on the horizontal axis.

<div align="center"><iframe src="https://www.desmos.com/calculator/j1thkiirwu?embed" width="500" height="250" style="border: 1px solid #ccc" frameborder=0></iframe></div>

## Function Intuitions
Functions can take many forms, since there are many real-world processes that are described by functions. It is therefore important to develop an intuition around some of the basic  functions. 

### Linear Functions

#### Simple Addition
Linear functions, such as $f(x) = x + x + x = 3x$ then show that for one input to the function ($f(x)$) we get three times that input, $3x$ as the output. 

<div align = "center">
<iframe src="https://www.desmos.com/calculator/tvyo4qth6u?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>
</div>


#### Equation of a Line
The equation of any line can be generalised to $f(x) = m \cdot x + c$ . Where for any input $x$ we output $m \cdot x$  and add $c$. 
- $m$ corresponds to the *gradient* of the line.
- $c$ corresponds to the *y intercept* of the line. 

<div align = "center">
<iframe src="https://www.desmos.com/calculator/mo305n5vx3" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>
</div>

### Non-linear Functions

#### Exponentials
Exponentials describe *how many times a variable is being multiplied by itself*. In other words, how many times are we *feeding back* a variable, for any given input of that variable. 

<div align="center"><iframe src="https://www.desmos.com/calculator/4vzfym1hcx?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=1000></iframe>
</div>


## Formal Definition of a Function
A function, formally, maps elements of one [[Sets|set]] to elements of another set. 
$$
f : X\rightarrow Y, \ \ \ \ x \rightarrow f(x)
$$
In most of the cases covered in the [[#Basic Definition|above section]], we are dealing with functions that map from [[Sets#^b8ba0f|real numbers]] to another set in the real numbers, in other words $f: \mathbb R \rightarrow \mathbb R$. However, many other types of functions exist. 
# Why do we need Functions?
Functions unlock a way of analysing the world that would have been previously hard to analyse concretely. If two things are related in some way, a function is needed. Whether it be how the speed of an object changes with time, how the number of trees relates to the amount of wildlife in a given area, or how spacecraft hurl around planets in the solar systems. Functions help us understand relationships.

---
# Functionals

^ccba29

## Basic Definition
Functionals are a generalisation of the behaviour of functions. There are different ways to define functionals, based on the domain in which they are applied. 

### 1. The [[Linear Algebra]] Definition


### 2. The Functional Analysis Definition

In functional analysis, functionals differ in subtle ways to functions, taking *functions as inputs to functionals*, and outputting a *scalar value* - a *number*. 
$$
\mathcal F[f] \rightarrow \mathbb R
$$

# Why do we need Functionals?

