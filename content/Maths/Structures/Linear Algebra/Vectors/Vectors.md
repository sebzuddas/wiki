---
aliases:
  - vector
  - Vector
  - vectors
  - Vectors
tags:
  - maths
  - engineering
  - maths
  - modeling
---
# What is a Vector?
Vectors are a useful way to group together related properties *that cannot be expressed by a single number*. Although a physics class teaches that a vector is 'something with both magnitude and direction', such a description leads to thinking about vectors in a purely mechanical sense. 

# Why do we need Vectors?
Vectors are particularly useful when representing a number of things. 
## Data
In the world of AI & [[Machine Learning]], vectors and [[Matrices|matrices]] are useful mathematical tools used to manipulate data. 

## Modelling
When [[System Modelling|modelling systems]], vectors are useful tools to 



# Chapter 1. Vectors

## 1.1 Scalars and Vectors

A scalar is a single numerical quantity, most often taken from the real numbers, denoted by $\mathbb{R}$. Scalars are the fundamental building blocks of arithmetic: they can be added, subtracted, multiplied, and, except in the case of zero, divided. In linear algebra, scalars play the role of coefficients, scaling factors, and entries of larger structures such as vectors and matrices. They provide the weights by which more complex objects are measured and combined. A vector is an ordered collection of scalars, arranged either in a row or a column. When the scalars are real
numbers, the vector is said to belong to *real* $n$-dimensional space, written

$$
\mathbb{R}^n = \{ (x_1, x_2, \dots, x_n) \mid x_i \in \mathbb{R} \}.
$$

An element of $\mathbb{R}^n$ is called a vector of dimension $n$ or an *n*-vector. The number $n$ is called the
dimension of the vector space. Thus $\mathbb{R}^2$ is the space of all ordered pairs of real numbers, $\mathbb{R}^3$ the
space of all ordered triples, and so on.

Example 1.1.1.

- A 2-dimensional vector: $(3, -1) \in \mathbb{R}^2$.
- A 3-dimensional vector: $(2, 0, 5) \in \mathbb{R}^3$.
- A 1-dimensional vector: $(7) \in \mathbb{R}^1$, which corresponds to the scalar $7$ itself.

Vectors are often written vertically in column form, which emphasises their role in matrix multiplication:

$$
\mathbf{v} = \begin{bmatrix} 2 \\ 0 \\ 5 \end{bmatrix} \in \mathbb{R}^3.
$$

The vertical layout makes the structure clearer when we consider linear combinations or multiply matrices by vectors.

### Geometric Interpretation

In $\mathbb{R}^2$, a vector $(x_1, x_2)$ can be visualised as an arrow starting at the origin $(0,0)$ and ending at the
point $(x_1, x_2)$. Its length corresponds to the distance from the origin, and its orientation gives a direction in the plane. In $\mathbb{R}^3$, the same picture extends into three dimensions: a vector is an arrow from the origin to $(x_1, x_2, x_3)$. Beyond three dimensions, direct visualisation is no longer possible, but the algebraic rules of vectors remain identical. Even though we cannot draw a vector in $\mathbb{R}^{10}$, it behaves under addition, scaling, and transformation exactly as a 2- or 3-dimensional vector does. This abstract point of view is what allows linear algebra to apply to data science, physics, and machine learning, where data often lives in very high-dimensional spaces. Thus a vector may be regarded in three complementary ways:

1. As a point in space, described by its coordinates.
2. As a displacement or arrow, described by a direction and a length.
3. As an abstract element of a vector space, whose properties follow algebraic rules independent of geometry.

### Notation

- Vectors are written in boldface lowercase letters: $\mathbf{v}, \mathbf{w}, \mathbf{x}$.
- The *i*-th entry of a vector $\mathbf{v}$ is written $v_i$, where indices begin at 1.
- The set of all *n*-dimensional vectors over $\mathbb{R}$ is denoted $\mathbb{R}^n$.
- Column vectors will be the default form unless otherwise stated.

### Why begin here?

Scalars and vectors form the atoms of linear algebra. Every structure we will build-vector spaces, linear transformations, [[matrices]], eigenvalues-relies on the basic notions of number and ordered collection of numbers. Once [[vectors]] are understood, we can define operations such as addition and scalar multiplication, then generalise to subspaces, bases, and coordinate systems. Eventually, this framework grows into the full theory of linear algebra, with powerful applications to [[geometry]], computation, and data.

### Exercises 1.1

1. Write three different vectors in $\mathbb{R}^2$ and sketch them as arrows from the origin. Identify their coordinates
   explicitly.
2. Give an example of a vector in $\mathbb{R}^4$. Can you visualize it directly? Explain why high-dimensional
   visualization is challenging.
3. Let $\mathbf{v} = (4, -3, 2)$. Write $\mathbf{v}$ in column form and state $v_1, v_2, v_3$.
4. In what sense is the set $\mathbb{R}^1$ both a line and a vector space? Illustrate with examples.
5. Consider the vector $\mathbf{u} = (1,1,\dots,1) \in \mathbb{R}^n$. What is special about this vector when $n$ is
   large? What might it represent in applications?

## 1.2 Vector Addition and Scalar Multiplication

Vectors in linear algebra are not static objects; their power comes from the operations we can perform on them. Two
fundamental operations define the structure of vector spaces: addition and scalar multiplication. These operations
satisfy simple but far-reaching rules that underpin the entire subject.

### Vector Addition

Given two vectors of the same dimension, their sum is obtained by adding corresponding entries. Formally, if

$$
\mathbf{u} = (u_1, u_2, \dots, u_n), \quad
\mathbf{v} = (v_1, v_2, \dots, v_n),
$$

then their sum is

$$
\mathbf{u} + \mathbf{v} = (u_1+v_1, u_2+v_2, \dots, u_n+v_n).
$$

Example 1.2.1.
Let $\mathbf{u} = (2, -1, 3)$ and $\mathbf{v} = (4, 0, -5)$. Then

$$
\mathbf{u} + \mathbf{v} = (2+4, -1+0, 3+(-5)) = (6, -1, -2).
$$

Geometrically, vector addition corresponds to the *parallelogram rule*. If we draw both vectors as arrows from the
origin, then placing the tail of one vector at the head of the other produces the sum. The diagonal of the parallelogram
they form represents the resulting vector.

### Scalar Multiplication

Multiplying a vector by a scalar stretches or shrinks the vector while preserving its direction, unless the scalar is negative, in which case the vector is also reversed. If $c \in \mathbb{R}$ and

$$
\mathbf{v} = (v_1, v_2, \dots, v_n),
$$

then

$$
c \mathbf{v} = (c v_1, c v_2, \dots, c v_n).
$$

Example 1.2.2. 
Let $\mathbf{v} = (3, -2)$ and $c = -2$. Then

$$
c\mathbf{v} = -2(3, -2) = (-6, 4).
$$

This corresponds to flipping the vector through the origin and doubling its length.

### Linear Combinations

The interaction of addition and scalar multiplication allows us to form *linear combinations*. A linear combination of
vectors $\mathbf{v}_1, \mathbf{v}_2, \dots, \mathbf{v}_k$ is any vector of the form

$$
c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \cdots + c_k \mathbf{v}_k, \quad c_i \in \mathbb{R}.
$$

Linear combinations are the mechanism by which we generate new vectors from existing ones. The span of a set of vectors-the collection of all their linear combinations-will later lead us to the idea of a subspace.

Example 1.2.3.
Let $\mathbf{v}_1 = (1,0)$ and $\mathbf{v}_2 = (0,1)$. Then any vector $(a,b)\in\mathbb{R}^2$ can be expressed as

$$
a\mathbf{v}_1 + b\mathbf{v}_2.
$$

Thus $(1,0)$ and $(0,1)$ form the basic building blocks of the plane.

### Notation

* Addition: $\mathbf{u} + \mathbf{v}$ means component-wise addition.
* Scalar multiplication: $c\mathbf{v}$ scales each entry of $\mathbf{v}$ by $c$.
* Linear combination: a sum of the form $c_1 \mathbf{v}_1 + \cdots + c_k \mathbf{v}_k$.

### Why this matters

Vector addition and scalar multiplication are the defining operations of linear algebra. They give structure to vector spaces, allow us to describe geometric phenomena like translation and scaling, and provide the foundation for solving systems of equations. Everything that follows basis, dimension, transformations-builds on these simple but profound rules.

### Exercises 1.2

1. Compute $\mathbf{u} + \mathbf{v}$ where $\mathbf{u} = (1,2,3)$ and $\mathbf{v} = (4, -1, 0)$.
2. Find $3\mathbf{v}$ where $\mathbf{v} = (-2,5)$. Sketch both vectors to illustrate the scaling.
3. Show that $(5,7)$ can be written as a linear combination of $(1,0)$ and $(0,1)$.
4. Write $(4,4)$ as a linear combination of $(1,1)$ and $(1,-1)$.
5. Prove that if $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$,
   then $(c+d)(\mathbf{u}+\mathbf{v}) = c\mathbf{u} + c\mathbf{v} + d\mathbf{u} + d\mathbf{v}$ for
   scalars $c,d \in \mathbb{R}$.

## 1.3 Dot Product, Norms, and Angles

The dot product is the fundamental operation that links algebra and geometry in vector spaces. It allows us to measure lengths, compute angles, and determine orthogonality. From this single definition flow the notions of *norm* and *angle*, which give geometry to abstract vector spaces.

### The Dot Product

For two vectors in $\mathbb{R}^n$, the dot product (also called the inner product) is defined by

$$
\mathbf{u} \cdot \mathbf{v} = u_1 v_1 + u_2 v_2 + \cdots + u_n v_n.
$$

Equivalently, in matrix notation:

$$
\mathbf{u} \cdot \mathbf{v} = \mathbf{u}^T \mathbf{v}.
$$

Example 1.3.1.
Let $\mathbf{u} = (2, -1, 3)$ and $\mathbf{v} = (4, 0, -2)$. Then

$$
\mathbf{u} \cdot \mathbf{v} = 2\cdot 4 + (-1)\cdot 0 + 3\cdot (-2) = 8 - 6 = 2.
$$

The dot product outputs a single scalar, not another vector.

### Norms (Length of a Vector)

The *Euclidean norm* of a vector is the square root of its dot product with itself:

$$
\|\mathbf{v}\| = \sqrt{\mathbf{v} \cdot \mathbf{v}} = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2}.
$$

This generalizes the Pythagorean theorem to arbitrary dimensions.

Example 1.3.2.
For $\mathbf{v} = (3, 4)$,

$$
\|\mathbf{v}\| = \sqrt{3^2 + 4^2} = \sqrt{25} = 5.
$$

This is exactly the length of the vector as an arrow in the plane.

### Angles Between Vectors

The dot product also encodes the angle between two vectors. For nonzero vectors $\mathbf{u}, \mathbf{v}$,

$$
\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\| \, \|\mathbf{v}\| \cos \theta,
$$

where $\theta$ is the angle between them. Thus,

$$
\cos \theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|\|\mathbf{v}\|}.
$$

Example 1.3.3.
Let $\mathbf{u} = (1,0)$ and $\mathbf{v} = (0,1)$. Then

$$
\mathbf{u} \cdot \mathbf{v} = 0, \quad \|\mathbf{u}\| = 1, \quad \|\mathbf{v}\| = 1.
$$

Hence

$$
\cos \theta = \frac{0}{1\cdot 1} = 0 \quad \Rightarrow \quad \theta = \frac{\pi}{2}.
$$

The vectors are perpendicular.

### Orthogonality

Two vectors are said to be orthogonal if their dot product is zero:

$$
\mathbf{u} \cdot \mathbf{v} = 0.
$$

Orthogonality generalizes the idea of perpendicularity from geometry to higher dimensions.

### Notation

* Dot product: $\mathbf{u} \cdot \mathbf{v}$.
* Norm (length): $|\mathbf{v}|$.
* Orthogonality: $\mathbf{u} \perp \mathbf{v}$ if $\mathbf{u} \cdot \mathbf{v} = 0$.

### Why this matters

The dot product turns vector spaces into geometric objects: vectors gain lengths, angles, and notions of perpendicularity. This foundation will later support the study of orthogonal projections, Gram–Schmidt orthogonalisation, eigenvectors, and least squares problems.

### Exercises 1.3

1. Compute $\mathbf{u} \cdot \mathbf{v}$ for $\mathbf{u} = (1,2,3)$, $\mathbf{v} = (4,5,6)$.
2. Find the norm of $\mathbf{v} = (2, -2, 1)$.
3. Determine whether $\mathbf{u} = (1,1,0)$ and $\mathbf{v} = (1,-1,2)$ are orthogonal.
4. Let $\mathbf{u} = (3,4)$, $\mathbf{v} = (4,3)$. Compute the angle between them.
5. Prove that $|\mathbf{u} + \mathbf{v}|^2 = |\mathbf{u}|^2 + |\mathbf{v}|^2 + 2\mathbf{u}\cdot \mathbf{v}$. This
   identity is the algebraic version of the Law of Cosines.

## 1.4 Orthogonality

Orthogonality captures the notion of perpendicularity in vector spaces. It is one of the most important geometric ideas in linear algebra, allowing us to decompose vectors, define projections, and construct special bases with elegant properties.

### Definition

Two vectors $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$ are said to be orthogonal if their dot product is zero:

$$
\mathbf{u} \cdot \mathbf{v} = 0.
$$

This condition ensures that the angle between them is $\pi/2$ radians (90 degrees).

Example 1.4.1.
In $\mathbb{R}^2$, the vectors $(1,2)$ and $(2,-1)$ are orthogonal since

$$
(1,2) \cdot (2,-1) = 1\cdot 2 + 2\cdot (-1) = 0.
$$

### Orthogonal Sets

A collection of vectors is called orthogonal if every distinct pair of vectors in the set is orthogonal. If, in addition, each vector has norm 1, the set is called orthonormal.

Example 1.4.2.
In $\mathbb{R}^3$, the standard basis vectors

$$
\mathbf{e}_1 = (1,0,0), \quad \mathbf{e}_2 = (0,1,0), \quad \mathbf{e}_3 = (0,0,1)
$$

form an orthonormal set: each has length 1, and their dot products vanish when the indices differ.

### Projections

Orthogonality makes possible the decomposition of a vector into two components: one parallel to another vector, and one orthogonal to it. Given a nonzero vector $\mathbf{u}$ and any vector $\mathbf{v}$, the projection of $\mathbf{v}$ onto $\mathbf{u}$ is

$$
\text{proj}_{\mathbf{u}}(\mathbf{v}) = \frac{\mathbf{u} \cdot \mathbf{v}}{\mathbf{u} \cdot \mathbf{u}} \mathbf{u}.
$$

The difference

$$
\mathbf{v} - \text{proj}_{\mathbf{u}}(\mathbf{v})
$$

is orthogonal to $\mathbf{u}$. Thus every vector can be decomposed uniquely into a parallel and perpendicular part with respect to another vector.

Example 1.4.3.
Let $\mathbf{u} = (1,0)$, $\mathbf{v} = (2,3)$. Then

$$
\text{proj}_{\mathbf{u}}(\mathbf{v}) = \frac{(1,0)\cdot(2,3)}{(1,0)\cdot(1,0)} (1,0)
= \frac{2}{1}(1,0) = (2,0).
$$

Thus

$$
\mathbf{v} = (2,3) = (2,0) + (0,3),
$$

where $(2,0)$ is parallel to $(1,0)$ and $(0,3)$ is orthogonal to it.

### Orthogonal Decomposition

In general, if $\mathbf{u} \neq \mathbf{0}$ and $\mathbf{v} \in \mathbb{R}^n$, then

$$
\mathbf{v} = \text{proj}_{\mathbf{u}}(\mathbf{v}) + \big(\mathbf{v} - \text{proj}_{\mathbf{u}}(\mathbf{v})\big),
$$

where the first term is parallel to $\mathbf{u}$ and the second term is orthogonal. This decomposition underlies methods such as least squares approximation and the Gram–Schmidt process.

### Notation

* $\mathbf{u} \perp \mathbf{v}$: vectors $\mathbf{u}$ and $\mathbf{v}$ are orthogonal.
* An orthogonal set: vectors pairwise orthogonal.
* An orthonormal set: pairwise orthogonal, each of norm 1.

### Why this matters

Orthogonality gives structure to vector spaces. It provides a way to separate independent directions cleanly, simplify computations, and minimise errors in approximations. Many powerful algorithms in numerical linear algebra and data science (QR decomposition, least squares regression, PCA) rely on orthogonality.

### Exercises 1.4

1. Verify that the vectors $(1,2,2)$ and $(2,0,-1)$ are orthogonal.
2. Find the projection of $(3,4)$ onto $(1,1)$.
3. Show that any two distinct standard basis vectors in $\mathbb{R}^n$ are orthogonal.
4. Decompose $(5,2)$ into components parallel and orthogonal to $(2,1)$.
5. Prove that if $\mathbf{u}, \mathbf{v}$ are orthogonal and nonzero,
   then $(\mathbf{u}+\mathbf{v})\cdot(\mathbf{u}-\mathbf{v}) = 0$.
