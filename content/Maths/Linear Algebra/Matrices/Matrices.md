---
aliases:
  - matrix
  - matrices
  - Matrix
  - Matrices
tags:
  - control
  - maths
  - data
  - engineering
  - modeling
  - system
---
Typically denoted by a capital letter $X$, a matrix with $m$ rows and $n$ columns is defined as $X\in \mathbb R^{m \times n}$. 

# Intuition
Although matrix can seem abstract, they have a real world context. In a simplistic explanation of matrices, we can think of the columns, $n$, as the inputs to the matrix, and the rows, $m$, as the outputs of the matrix. For example, let's take a simple $A\in \mathbb R^{2\times 2}$ matrix. This corresponds to a system with two inputs and two outputs. 
$$
A=\begin{matrix}
 a_{11} & a_{12} \\
 a_{21} & a_{22} \\
\end{matrix}
$$
The values on the leading diagonal, those being $a_{11}$ and $a_{22}$, describe the relationships of the inputs themselves. The other values, $a_{21}$ and $a_{12}$ describe the relationships between the *two different* inputs. This is why using particular analytical techniques involving finding eigenvalues of a matrix becomes so useful, since it describes the fundamental underpinnings of a given system. 

%% 
~~Another way of framing it would be to consider all inputs as $a$ and all outputs as $z$. Then the matrix A would look like this:~~
A=\begin{matrix}
 a_{11} & a_{12} \\
 a_{21} & a_{22} \\
\end{matrix}
%% 

# Matrix Rules
1. Additivity
2. Commutativity
3. Multiplicity

# Matrix Addition

# Matrix Multiplication

For two given matrices $A \in \mathbb{R}^{m_1 \times n_1}$  and $B \in \mathbb{R}^{m_2 \times n_2}$ we need to have $n_1 = m_2$. In other words, the number of columns of the left hand matrix $A$ need to match the number of columns in the right hand matrix $B$. The resulting matrix, $D$ is given by $D \in \mathbb{R}^{m_1 \times n_2}$. 

If we have $A$ and $B$ as:

$$
A=\begin{pmatrix}
 a_{11} & a_{12} & \cdots & a_{1n_1} \\
 a_{21} & a_{22} & \cdots & a_{2n_1} \\
 \vdots & \vdots & \ddots & \vdots \\
 a_{m_11} & a_{m_12} & \cdots & a_{m_1n_1} \\
\end{pmatrix},\quad B=\begin{pmatrix}
 b_{11} & b_{12} & \cdots & b_{1m_2} \\
 b_{21} & b_{22} & \cdots & b_{2m_2} \\
 \vdots & \vdots & \ddots & \vdots \\
 b_{n_21} & b_{n_22} & \cdots & b_{n_2m_2} \\
\end{pmatrix}
$$
And if $A \times B = D$ we get:

$$
D = \begin{pmatrix}
 a_{11}b_{11} +\cdots + a_{1n}b_{n1} & a_{11}b_{12} +\cdots + a_{1n}b_{n2} & \cdots & a_{11}b_{1p} +\cdots + a_{1n}b_{np} \\
 a_{21}b_{11} +\cdots + a_{2n}b_{n1} & a_{21}b_{12} +\cdots + a_{2n}b_{n2} & \cdots & a_{21}b_{1p} +\cdots + a_{2n}b_{np} \\
\vdots & \vdots & \ddots & \vdots \\
 a_{m1}b_{11} +\cdots + a_{mn}b_{n1} & a_{m1}b_{12} +\cdots + a_{mn}b_{n2} & \cdots & a_{m1}b_{1p} +\cdots + a_{mn}b_{np} \\
\end{pmatrix} 

$$

### Example 1: Multiplying a $3 \times 3$ matrix by a $3 \times 1$  vector

Given:
$$
A = \begin{pmatrix} \color{red}{1} & \color{lightblue}{2} & \color{green}{3} \\ \color{red}{4} & \color{lightblue}{5} & \color{green}{6} \\ \color{red}{7} & \color{lightblue}{8} & \color{green}{9} \\ \end{pmatrix}, \quad B = \begin{pmatrix} \color{red}{1} \\ \color{lightblue}{0} \\ \color{green}{-1} \\ \end{pmatrix}​​
$$
We calculate $A \times B=D$ by multiplying each row of $A$ by the column vector $B$:

$$
D = \begin{pmatrix} \color{red}{1}\times \color{red}{1} + \color{lightblue}{2}\times \color{lightblue}{0} + \color{green}{3}\times \color{green}{-1} \\ \color{red}{4}\times \color{red}{1} + \color{lightblue}{5}\times \color{lightblue}{0} + \color{green}{6}\times \color{green}{-1} \\ \color{red}{7}\times \color{red}{1} + \color{lightblue}{8}\times \color{lightblue}{0} + \color{green}{9}\times \color{green}{-1} \end{pmatrix} = \begin{pmatrix} \color{red}{1} + \color{lightblue}{0} + \color{green}{-3} \\ \color{red}{4} + \color{lightblue}{0} + \color{green}{-6} \\ \color{red}{7} + \color{lightblue}{0} + \color{green}{-9} \end{pmatrix} = \begin{pmatrix} -2 \\ -2 \\ -2 \end{pmatrix}​​
$$

Here, each row in $A$ is multiplied element-by-element with the single column in $B$. The colour coding helps track how each corresponding element from the row and column is used in the multiplication.

---

### Example 2: Multiplying a $3 \times 3$  matrix by a $3 \times 2$ matrix

Given:

$$
A = \begin{pmatrix} \color{red}{1} & \color{blue}{2} & \color{green}{3} \\ 
\color{red}{4} & \color{blue}{5} & \color{green}{6} \\ 
\color{red}{7} & \color{blue}{8} & \color{green}{9} 
\end{pmatrix}, \quad 
B = \begin{pmatrix} \color{red}{1} & \color{orange}{0} \\ 
\color{blue}{0} & \color{pink}{1} \\ 
\color{green}{-1} & \color{brown}{0} 
\end{pmatrix}
$$

We calculate $A \times B=D$ by multiplying each row of $A$ by each column of $B$:

$$
D = \begin{pmatrix} 
(\color{red}{1}\times \color{red}{1} + \color{blue}{2}\times \color{blue}{0} + \color{green}{3}\times \color{green}{-1}) & 
(\color{red}{1}\times \color{orange}{0} + \color{blue}{2}\times \color{pink}{1} + \color{green}{3}\times \color{brown}{0}) \\ 
(\color{red}{4}\times \color{red}{1} + \color{blue}{5}\times \color{blue}{0} + \color{green}{6}\times \color{green}{-1}) & 
(\color{red}{4}\times \color{orange}{0} + \color{blue}{5}\times \color{pink}{1} + \color{green}{6}\times \color{brown}{0}) \\ 
(\color{red}{7}\times \color{red}{1} + \color{blue}{8}\times \color{blue}{0} + \color{green}{9}\times \color{green}{-1}) & 
(\color{red}{7}\times \color{orange}{0} + \color{blue}{8}\times \color{pink}{1} + \color{green}{9}\times \color{brown}{0}) 
\end{pmatrix}
$$

This simplifies to:

$$
D = \begin{pmatrix} 
(\color{red}{1} + \color{blue}{0} + \color{green}{-3}) & (\color{red}{0} + \color{blue}{2} + \color{green}{0}) \\ 
(\color{red}{4} + \color{blue}{0} + \color{green}{-6}) & (\color{red}{0} + \color{blue}{5} + \color{green}{0}) \\ 
(\color{red}{7} + \color{blue}{0} + \color{green}{-9}) & (\color{red}{0} + \color{blue}{8} + \color{green}{0}) 
\end{pmatrix} 
= \begin{pmatrix} -2 & 2 \\ -2 & 5 \\ -2 & 8 \end{pmatrix}
$$

---

### Example 3: Multiplying two $3 \times 3$  matrices

Given:

$$
A = \begin{pmatrix} \color{red}{1} & \color{blue}{2} & \color{green}{3} \\ 
\color{red}{4} & \color{blue}{5} & \color{green}{6} \\ 
\color{red}{7} & \color{blue}{8} & \color{green}{9} 
\end{pmatrix}, \quad 
B = \begin{pmatrix} \color{red}{1} & \color{orange}{0} & \color{pink}{1} \\ 
\color{blue}{0} & \color{brown}{1} & \color{pink}{0} \\ 
\color{green}{-1} & \color{cyan}{0} & \color{olive}{1} 
\end{pmatrix}
$$

We calculate $A \times B=D$ by multiplying each row of $A$ by each column of $B$:

$$
D = \begin{pmatrix} 
(\color{red}{1}\times \color{red}{1} + \color{blue}{2}\times \color{blue}{0} + \color{green}{3}\times \color{green}{-1}) & 
(\color{red}{1}\times \color{orange}{0} + \color{blue}{2}\times \color{brown}{1} + \color{green}{3}\times \color{cyan}{0}) & 
(\color{red}{1}\times \color{pink}{1} + \color{blue}{2}\times \color{pink}{0} + \color{green}{3}\times \color{olive}{1}) \\ 
(\color{red}{4}\times \color{red}{1} + \color{blue}{5}\times \color{blue}{0} + \color{green}{6}\times \color{green}{-1}) & 
(\color{red}{4}\times \color{orange}{0} + \color{blue}{5}\times \color{brown}{1} + \color{green}{6}\times \color{cyan}{0}) & 
(\color{red}{4}\times \color{pink}{1} + \color{blue}{5}\times \color{pink}{0} + \color{green}{6}\times \color{olive}{1}) \\ 
(\color{red}{7}\times \color{red}{1} + \color{blue}{8}\times \color{blue}{0} + \color{green}{9}\times \color{green}{-1}) & 
(\color{red}{7}\times \color{orange}{0} + \color{blue}{8}\times \color{brown}{1} + \color{green}{9}\times \color{cyan}{0}) & 
(\color{red}{7}\times \color{pink}{1} + \color{blue}{8}\times \color{pink}{0} + \color{green}{9}\times \color{olive}{1}) 
\end{pmatrix}
$$

This simplifies to:

$$
D = \begin{pmatrix} 
(\color{red}{1} + \color{blue}{0} + \color{green}{-3}) & (\color{red}{0} + \color{blue}{2} + \color{green}{0}) & (\color{red}{1} + \color{blue}{0} + \color{green}{3}) \\ 
(\color{red}{4} + \color{blue}{0} + \color{green}{-6}) & (\color{red}{0} + \color{blue}{5} + \color{green}{0}) & (\color{red}{4} + \color{blue}{0} + \color{green}{6}) \\ 
(\color{red}{7} + \color{blue}{0} + \color{green}{-9}) & (\color{red}{0} + \color{blue}{8} + \color{green}{0}) & (\color{red}{7} + \color{blue}{0} + \color{green}{9}) 
\end{pmatrix} 
= \begin{pmatrix} -2 & 2 & 4 \\ -2 & 5 & 10 \\ -2 & 8 & 16 \end{pmatrix}
$$





# Matrix Algebra

## Matrix Inverse
The inverse of a given matrix $A$ exists if and only if $A \in \mathbb{R}^{n\times n}$ (in other words square) and if the leading diagonal is not 0.  

Broadly, the steps for finding the inverse are as follows for a matrix higher than $\mathbb{R}^{m\times m}$:

$$ Y= \Phi \beta \\ \therefore \\ \beta=\Phi^{-1}Y $$

As such, the inverse is found by:

$$ A^{-1}=\frac{1}{|A|} \cdot C_A^T $$

For a $\mathbb{R}^{2\times2}$ matrix there is a shortcut:

$$
\begin{pmatrix} a & b \\ c & d \end{pmatrix}^{-1}
= \frac{1}{ad-bc}
\begin{pmatrix} d & -b \\ -c & a \end{pmatrix}
$$

## Pseudoinverse
^8a05b2
Used when we need to invert a matrix but the matrix isn't square, i.e. $\mathbb{R}^{m\times n}$.

$$
\Phi_p^{-1} = (\Phi^{T}\Phi)^{-1}\,\Phi^{T}
$$

