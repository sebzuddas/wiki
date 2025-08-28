The $\mathcal Z$-Transform plays the same role as the Laplace Transform in the discrete domain $z$.

$$ x[n]\overbrace{\implies}^{\mathcal Z}\mathcal X(z) $$

The Bilateral $\mathcal Z$-Transform is given by:

$$ \mathcal{X}(z)=\sum_{n=-\infty}^{\infty}x[n]\cdot z^{-n}\\

\text{for }z\in \mathbb{C} $$

The Unilateral $\mathcal Z$-Transform is given by:

$$ \color{green}\mathcal{X}(z)=\sum_{n=0}^{\infty}x[n]\cdot z^{-n}\\

\text{for }z\in \mathbb{C} $$

The $\mathcal Z$-transform of a signal $x[n]$ gives a difference equation as follows:

$$ \mathcal{X}(z)=x[0]z^{-0}+x[1]z^{-1}+...+x[n]z^{-n} $$

All of the values of $z$ make the summation exist in a Region of Convergence (RoC).

## Complex Variable $z$

$$ z=re^{j\omega}=r\cos(\omega)+j\sin(\omega) $$

where:

- $r$ is the rate of decay or growth.
- $\omega$ is the frequency of oscillation.

## Properties

For a system impulse response $h(n)$ its Z-transform is given by:

$\mathcal H(z)=\sum_{n=-\infty}^{\infty}h(n)\cdot z^{-n}$

Similarly, the Z-transform of a scaled delta function $a\delta(n)$ is given by:

$\mathcal H(z)=\sum_{n=-\infty}^{\infty}a\cdot\delta(n)\cdot z^{-n}=a$

$$ x(n)\overbrace{\iff}^{\mathcal Z}\mathcal X(z) $$

- Linearity: $ax(n)+by(n) \underbrace{\iff}_{\mathcal Z} a\mathcal X(z)+b\mathcal Y(z)$
- Delay: $x(n-n_0) \underbrace{\iff}_{\mathcal Z} z^{-n_0}\mathcal X(z)$
- Convolution: $y(n)=h(n)*x(n) \underbrace{\iff}_{\mathcal Z}\mathcal Y(z)=\mathcal H(z)\cdot \mathcal X(z)$

### Simple Examples of $\mathcal Z$-Transforms

1. Finite-length Sequence
    
    $$ \begin{bmatrix} 1&2&-1&3 \end{bmatrix} \overbrace{\iff}^{\mathcal Z}
    
    1+2z^{-1}-1z^{-2}+3z^{-3} $$
    
2. Impulse
    
    $$ \delta(n)\overbrace{\iff}^{\mathcal Z} \sum_{n=-\infty}^{\infty}\delta(n)z^{-n}=z^{-0}=1 $$