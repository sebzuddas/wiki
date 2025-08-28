# Terminology

### Old -> New Terminology

- Crisp set -> Fuzzy Set
- Characteristic equation -> Membership Function
- Mapping -> Universal Set


A universal set refers to _all objects_ that one wishes to describe, eg. the universal set of room temperatures (low, temperate, high, very high,...).

Consider a set $A$ of which $A \subset X$, for $X$ is a universal set.

Boolean logic infers that each element of $A$, $\mu_A=1 \vee 0$ , i.e. objects in $X$ either belong in $A$ or not.

However, fuzzy logic uses _membership functions_ (MF), whereby each element in $A$ is has 'degrees of belonging' to the set $X$.

Hence, $\mu_A=[0,1]$ ie can take on the whole interval between 0 and 1. As such, the set $A$ is the fuzzy set.

---

# Fuzzy Set Definition

For a given universal set

$$ X=[x_1,x_2,...,x_n] $$

We formally define a fuzzy set $A$ as

$$ A=\begin{cases} (x,\mu_A(x)) & with \hspace{0.25cm}x\in X \\ and \hspace{0.25cm} 0 \le \mu_A(x) \le 1& \end{cases} $$

Note, certainty in this context is _NOT_ probability. This is because they're not linked with distributions. Instead, cartesian representations are used.

## Example of a fuzzy set from a universal set

$X = Indoor\hspace{0.1cm} temperatures$

$A = High \hspace{0.1cm} indoor\hspace{0.1cm} temperatures$

$$ X=[0\degree,10\degree,15\degree,20\degree,25\degree,35\degree] $$

Now, we create a sub (fuzzy) set of $X$ called $A$, which evaluates the values in $X$ and grades them on whether they count as 'high indoor temperatures'

$$ A=[\frac{0}{0\degree},\frac{0.4}{10\degree},\frac{0.6}{15\degree},\frac{0.8}{20\degree},\frac{1}{25\degree},\frac{1}{35\degree}] $$

The values in the numerator of each element in $A$ correspond to how much the denominator belongs in $A$. In this case 0 doesn't count at all as a 'high indoor temperature', and 35 is certainly a 'high indoor temperature', hence they take on the values 0 and 1 respectively.

So, what dictates the values? How do we determine how much a value belongs in a fuzzy set from a universal set? We use [membership functions](https://www.notion.so/Fuzzy-Sets-75e1b3dcd32f4796a37d8dc6549da023?pvs=21). These are used to answer the question: _To what degrees do the values in $X$ belong in the fuzzy set $A$?_

[https://www.desmos.com/calculator/jemofb9bhj](https://www.desmos.com/calculator/jemofb9bhj)

Where $\mu_A(x)= FuzzyMF$

---

# The $\alpha$-Cut Set

- Useful to reduce the number of elements contained in the original set.
- Using the $\alpha-cut$ set leads to a reduction in mathematical operations.
- Useful when using Type 2-Fuzzy sets and when Z-slicing.

[https://www.desmos.com/calculator/7hr3hxnw8p](https://www.desmos.com/calculator/7hr3hxnw8p)

The $\alpha-cut$ set is the interval seen in green.

The support set is defined as:

$$ S(A)=\begin{cases}

x \in X,\ \mu_A(x)>0

\end{cases} $$

As such, the $\alpha-cut$ set is defined as:

$$ \alpha-cut(A) \subset A= \begin{cases}

x \in X,\ \mu_A(x)\ge \alpha , 0\le \alpha \le 1

\end{cases} $$

### Example of an $\alpha-cut$ set being applied

$$ A=\begin{cases} \frac{0}{1},\frac{0.25}{3},\frac{0.5}{4},\frac{0.7}{5},\frac{1}{8} \end{cases} \\ $$

We then apply an $\alpha$ value of 0.25, hence

$$ 0.25cut(A)=\begin{cases} \frac{1}{3},\frac{1}{4},\frac{1}{5},\frac{1}{8} \end{cases} $$

We see that all the values with numerators above that of 0.25 are included in the alpha cut set. Their membership in this set is definite, hence it is 1 for all these values.

---

# Typical MF Shapes

## Triangular

## Trapezoidal

## Non-Linear (Gaussian)

---

# Resulting Use Cases

Fuzzy logic has potential applications in many areas outside engineering.

### Finance

Fuzzy logic has the potential to be (and is often) applied in financial settings. The world of finance can be described as ‘the ideal scenario’ for fuzzy logic applications. This is because of the reasoning that is emergent from ‘IF, THEN’ statements. **Inputs** (such as $APPL, $FTSE100, etc) ie. current stock value can be then put through a fuzzy logic system which looks at **abstract ideas** such as: Company management structure, 5Y financial outlook, Annual trends and CEO’s statement. The fuzzy logic system would then output whether it is reasonable to buy or sell a the related stock. As for the CEO’s statement example, it is possible to analyse this through fuzzy logic as ‘positive, not so positive, negative, rather negative’. **Uncertainties** in this context may be represented as product or service failures/successes, scandal(s), descriptions of key members of the company, etc.

### Healthcare

