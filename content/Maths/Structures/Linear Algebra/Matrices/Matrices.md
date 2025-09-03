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
# What is a Matrix?
Typically denoted by a capital letter $X$, a matrix with $m$ rows and $n$ columns is defined as $X\in \mathbb R^{m \times n}$. 

# Why do we need Matrices?


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

---
# Eigenvectors


## Visualisation Tool
<div align="center">
<iframe src="https://www.desmos.com/calculator/wccoutuoxh" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>
</div>
# Eigenvalues


# 2. Matrices

## 2.1 Definition and Notation

Matrices are the central objects of linear algebra, providing a compact way to represent and manipulate linear transformations, systems of equations, and structured data. A matrix is a rectangular array of numbers arranged in rows and columns.

### Formal Definition

An $m \times n$ matrix is an array with $m$ rows and $n$ columns, written

$$
A =
\begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}.
$$

Each entry $a_{ij}$ is a scalar, located in the *i*-th row and *j*-th column. The size (or dimension) of the matrix is
denoted by $m \times n$.

* If $m = n$, the matrix is square.
* If $m = 1$, the matrix is a row vector.
* If $n = 1$, the matrix is a column vector.

Thus, vectors are simply special cases of matrices.

### Examples

Example 2.1.1. A $2 \times 3$ matrix:

$$
A = \begin{bmatrix}
1 & -2 & 4 \\
0 & 3 & 5
\end{bmatrix}.
$$

Here, $a_{12} = -2$, $a_{23} = 5$, and the matrix has 2 rows, 3 columns.

Example 2.1.2. A $3 \times 3$ square matrix:

$$
B = \begin{bmatrix}
2 & 0 & 1 \\
-1 & 3 & 4 \\
0 & 5 & -2
\end{bmatrix}.
$$

This will later serve as the representation of a linear transformation on $\mathbb{R}^3$.

### Indexing and Notation

* Matrices are denoted by uppercase bold letters: $A, B, C$.
* Entries are written as $a_{ij}$, with the row index first, column index second.
* The set of all real $m \times n$ matrices is denoted $\mathbb{R}^{m \times n}$.

Thus, a matrix is a function $A: {1,\dots,m} \times {1,\dots,n} \to \mathbb{R}$, assigning a scalar to each row-column
position.

### Why this matters

Matrices generalize vectors and give us a language for describing linear operations systematically. They encode systems of equations, rotations, projections, and transformations of data. With matrices, algebra and geometry come together: a single compact object can represent both numerical data and functional rules.

### Exercises 2.1

1. Write a $3 \times 2$ matrix of your choice and identify its entries $a_{ij}$.
2. Is every vector a matrix? Is every matrix a vector? Explain.
3. Which of the following are square
   matrices: $A \in \mathbb{R}^{4\times4}$, $B \in \mathbb{R}^{3\times5}$, $C \in \mathbb{R}^{1\times1}$?
4. Let $D = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$. What kind of matrix is this?
5. Consider the matrix $E = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$. Express $e_{11}, e_{12}, e_{21}, e_{22}$
   explicitly.

## 2.2 Matrix Addition and Multiplication

Once matrices are defined, the next step is to understand how they combine. Just as vectors gain meaning through addition and scalar multiplication, matrices become powerful through two operations: addition and multiplication.

### Matrix Addition

Two matrices of the same size are added by adding corresponding entries. If

$$
A = [a_{ij}] \in \mathbb{R}^{m \times n}, \quad
B = [b_{ij}] \in \mathbb{R}^{m \times n},
$$

then

$$
A + B = [a_{ij} + b_{ij}] \in \mathbb{R}^{m \times n}.
$$

Example 2.2.1.
Let

$$
A = \begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}, \quad
B = \begin{bmatrix}
-1 & 0 \\
5 & 2
\end{bmatrix}.
$$

Then

$$
A + B = \begin{bmatrix}
1 + (-1) & 2 + 0 \\
3 + 5 & 4 + 2
\end{bmatrix}
=
\begin{bmatrix}
0 & 2 \\
8 & 6
\end{bmatrix}.
$$

Matrix addition is commutative ($A+B = B+A$) and associative ($(A+B)+C = A+(B+C)$). The zero matrix, with all entries 0, acts as the additive identity.

### Scalar Multiplication

For a scalar $c \in \mathbb{R}$ and a matrix $A = [[a_{ij}]$, we define

$$
cA = [c \cdot a_{ij}].
$$

This stretches or shrinks all entries of the matrix uniformly.

Example 2.2.2.
If

$$
A = \begin{bmatrix}
2 & -1 \\
0 & 3
\end{bmatrix}, \quad c = -2,
$$

then

$$
cA = \begin{bmatrix}
-4 & 2 \\
0 & -6
\end{bmatrix}.
$$

### Matrix Multiplication

The defining operation of matrices is multiplication. If

$$
A \in \mathbb{R}^{m \times n}, \quad B \in \mathbb{R}^{n \times p},
$$

then their product is the $m \times p$ matrix

$$
AB = C = [c_{ij}], \quad c_{ij} = \sum_{k=1}^n a_{ik} b_{kj}.
$$

Thus, the entry in the $i$-th row and $j$-th column of $AB$ is the dot product of the $i$-th row of $A$ with the $j$-th column of $B$.

Example 2.2.3.
Let

$$
A = \begin{bmatrix}
1 & 2 \\
0 & 3
\end{bmatrix}, \quad
B = \begin{bmatrix}
4 & -1 \\
2 & 5
\end{bmatrix}.
$$

Then

$$
AB = \begin{bmatrix}
1\cdot4 + 2\cdot2 & 1\cdot(-1) + 2\cdot5 \\
0\cdot4 + 3\cdot2 & 0\cdot(-1) + 3\cdot5
\end{bmatrix}
=
\begin{bmatrix}
8 & 9 \\
6 & 15
\end{bmatrix}.
$$

Notice that matrix multiplication is not commutative in general: $AB \neq BA$. Sometimes $BA$ may not even be defined if dimensions do not align.

### Geometric Meaning

Matrix multiplication corresponds to the composition of linear transformations. If $A$ transforms vectors in $\mathbb{R}^n$ and $B$ transforms vectors in $\mathbb{R}^p$, then $AB$ represents applying $B$ first, then $A$. This
makes matrices the algebraic language of transformations.

### Notation

* Matrix sum: $A+B$.
* Scalar multiple: $cA$.
* Product: $AB$, defined only when the number of columns of $A$ equals the number of rows of $B$.

### Why this matters

Matrix multiplication is the core mechanism of linear algebra: it encodes how transformations combine, how systems of equations are solved, and how data flows in modern algorithms. Addition and scalar multiplication make matrices into a vector space, while multiplication gives them an algebraic structure rich enough to model [[geometry]], computation, and [[networks]].

### Exercises 2.2

1. Compute $A+B$ for

$$
A = \begin{bmatrix} 2 & 3 \\ -1 & 0 \end{bmatrix}, \quad
B = \begin{bmatrix} 4 & -2 \\ 5 & 7 \end{bmatrix}.
$$

2. Find $3A$ where

$$
A = \begin{bmatrix} 1 & -4 \\ 2 & 6 \end{bmatrix}.
$$

3. Multiply

$$
A = \begin{bmatrix} 1 & 0 & 2 \\ -1 & 3 & 1 \end{bmatrix}, \quad
B = \begin{bmatrix} 2 & 1 \\ 0 & -1 \\ 3 & 4 \end{bmatrix}.
$$

4. Verify with an explicit example that $AB \neq BA$.
5. Prove that matrix multiplication is distributive: $A(B+C) = AB + AC$.

## 2.3 Transpose and Inverse

Two special operations on matrices-the transpose and the inverse-give rise to deep algebraic and geometric properties. The transpose rearranges a matrix by flipping it across its main diagonal, while the inverse, when it exists, acts as the undo operation for matrix multiplication.

### The Transpose

The transpose of an $m \times n$ matrix $A = [a_{ij}]$ is the $n \times m$ matrix $A^T = [a_{ji}]$, obtained by swapping
rows and columns.

Formally,

$$
(A^T)_{ij} = a_{ji}.
$$

Example 2.3.1.
If

$$
A = \begin{bmatrix}
1 & 4 & -2 \\
0 & 3 & 5
\end{bmatrix},
$$

then

$$
A^T = \begin{bmatrix}
1 & 0 \\
4 & 3 \\
-2 & 5
\end{bmatrix}.
$$

Properties of the Transpose.

1. $(A^T)^T = A$.
2. $(A+B)^T = A^T + B^T$.
3. $(cA)^T = cA^T$, for scalar $c$.
4. $(AB)^T = B^T A^T$.

The last rule is crucial: the order reverses.

### The Inverse

A square matrix $A \in \mathbb{R}^{n \times n}$ is said to be invertible (or nonsingular) if there exists another matrix $A^{-1}$ such that

$$
AA^{-1} = A^{-1}A = I_n,
$$

where $I_n$ is the $n \times n$ identity matrix. In this case, $A^{-1}$ is called the inverse of $A$.

Not every matrix is invertible. A necessary condition is that $\det(A) \neq 0$, a fact that will be developed in [[#Chapter 6]].

Example 2.3.2.
Let

$$
A = \begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}.
$$

Its determinant is $\det(A) = (1)(4) - (2)(3) = -2 \neq 0$. The inverse is

$$
A^{-1} = \frac{1}{\det(A)} \begin{bmatrix}
4 & -2 \\
-3 & 1
\end{bmatrix}
=
\begin{bmatrix}
-2 & 1 \\
1.5 & -0.5
\end{bmatrix}.
$$

Verification:

$$
AA^{-1} = \begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
\begin{bmatrix}
-2 & 1 \\
1.5 & -0.5
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}.
$$

### Geometric Meaning

* The transpose corresponds to reflecting a linear transformation across the diagonal. For vectors, it switches between row and column forms.
* The inverse, when it exists, corresponds to reversing a linear transformation. For example, if $A$ scales and rotates vectors, $A^{-1}$ rescales and rotates them back.

### Notation

* Transpose: $A^T$.
* Inverse: $A^{-1}$, defined only for invertible square matrices.
* Identity: $I_n$, acts as the multiplicative identity.

### Why this matters

The transpose allows us to define symmetric and orthogonal matrices, central to geometry and numerical methods. The inverse underlies the solution of linear systems, encoding the idea of undoing a transformation. Together, these operations set the stage for determinants, eigenvalues, and orthogonalisation.

### Exercises 2.3

1. Compute the transpose of

$$
A = \begin{bmatrix} 2 & -1 & 3 \\ 0 & 4 & 5 \end{bmatrix}.
$$

2. Verify that $(AB)^T = B^T A^T$ for

$$
A = \begin{bmatrix} 1 & 2 \\ 0 & 1 \end{bmatrix}, \quad
B = \begin{bmatrix} 3 & 4 \\ 5 & 6 \end{bmatrix}.
$$

3. Determine whether

$$
C = \begin{bmatrix} 2 & 1 \\ 4 & 2 \end{bmatrix}
$$

is invertible. If so, find $C^{-1}$.

4. Find the inverse of

$$
D = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix},
$$

and explain its geometric action on vectors in the plane.

5. Prove that if $A$ is invertible, then so is $A^T$, and $(A^T)^{-1} = (A^{-1})^T$.

## 2.4 Special Matrices

Certain matrices occur so frequently in theory and applications that they are given special names. Recognising their properties allows us to simplify computations and understand the structure of linear transformations more clearly.

### The Identity Matrix

The identity matrix $I_n$ is the $n \times n$ matrix with ones on the diagonal and zeros elsewhere:

$$
I_n = \begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}.
$$

It acts as the multiplicative identity:

$$
AI_n = I_nA = A, \quad \text{for all } A \in \mathbb{R}^{n \times n}.
$$

Geometrically, $I_n$ represents the transformation that leaves every vector unchanged.

### Diagonal Matrices

A diagonal matrix has all off-diagonal entries zero:

$$
D = \begin{bmatrix}
d_{11} & 0 & \cdots & 0 \\
0 & d_{22} & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & d_{nn}
\end{bmatrix}.
$$

Multiplication by a diagonal matrix scales each coordinate independently:

$$
D\mathbf{x} = (d_{11}x_1, d_{22}x_2, \dots, d_{nn}x_n).
$$

Example 2.4.1.
Let

$$
D = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & -1 \end{bmatrix}, \quad
\mathbf{x} = \begin{bmatrix} 1 \\ 4 \\ -2 \end{bmatrix}.
$$

Then

$$
D\mathbf{x} = \begin{bmatrix} 2 \\ 12 \\ 2 \end{bmatrix}.
$$

### Permutation Matrices

A permutation matrix is obtained by permuting the rows of the identity matrix. Multiplying a vector by a permutation matrix reorders its coordinates.

Example 2.4.2.
Let

$$
P = \begin{bmatrix}
0 & 1 & 0 \\
1 & 0 & 0 \\
0 & 0 & 1
\end{bmatrix}.
$$

Then

$$
P\begin{bmatrix} a \\ b \\ c \end{bmatrix} =
\begin{bmatrix} b \\ a \\ c \end{bmatrix}.
$$

Thus, $P$ swaps the first two coordinates.

Permutation matrices are always invertible; their inverses are simply their transposes.

### Symmetric and Skew-Symmetric Matrices

A matrix is symmetric if

$$
A^T = A,
$$

and skew-symmetric if

$$
A^T = -A.
$$

Symmetric matrices appear in quadratic forms and [[Optimisation|optimisation]], while skew-symmetric matrices describe rotations and cross products in geometry.

### Orthogonal Matrices

A square matrix $Q$ is orthogonal if

$$
Q^T Q = QQ^T = I.
$$

Equivalently, the rows (and columns) of $Q$ form an orthonormal set. Orthogonal matrices preserve lengths and angles; they represent rotations and reflections.

Example 2.4.3.
The rotation matrix in the plane:

$$
R(\theta) = \begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix}
$$

is orthogonal, since

$$
R(\theta)^T R(\theta) = I_2.
$$

### Why this matters

Special matrices serve as the building blocks of linear algebra. Identity matrices define the neutral element, diagonal matrices simplify computations, permutation matrices reorder data, symmetric and orthogonal matrices describe fundamental geometric structures. Much of modern applied mathematics reduces complex problems to operations involving these simple forms.

### Exercises 2.4

1. Show that the product of two diagonal matrices is diagonal, and compute an example.
2. Find the permutation matrix that cycles $(a,b,c)$ into $(b,c,a)$.
3. Prove that every permutation matrix is invertible and its inverse is its transpose.
4. Verify that

$$
Q = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}
$$

is orthogonal. What geometric transformation does it represent?
5\. Determine whether

$$
A = \begin{bmatrix} 2 & 3 \\ 3 & 2 \end{bmatrix}, \quad
B = \begin{bmatrix} 0 & 5 \\ -5 & 0 \end{bmatrix}
$$

are symmetric, skew-symmetric, or neither.
