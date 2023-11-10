---
icon: circle
---

# W7 - Convex Optimization

## Definitions

!!!info 

##### ****Def.**** Convex Set

A set $S\sub\R^n$ is convex if for all $\bold{x,y}\in S,\lambda\in[0,1]$, the line connecting $\bold{x,y}$ is also in $S$.

$$
\lambda \bold x + (1-\lambda)\bold y \in S
$$

---

##### ****Def.**** Convex Function

A function $f:\R^n\to \R$ is convex iff the epigraph of $f$ (region above $f$) is convex.

For all $\bold{x,y}\in\text{Domain}(f), \lambda\in[0,1]$,

$$
f(\lambda\bold x + (1-\lambda)\bold y)\leqslant \lambda f(\bold{x}) + (1-\lambda)f(\bold{y})
$$

If $-f$ is convex, then $f$ is concave.

!!!

=== **Example.** Convex Functions

Let the domain be $(-\infty, \infty)$,

- $f(x) = e^x, f(x) = x^2$ are convex

- $f(x) = \sqrt{x}, f(x) = x^3$ are not convex

===

!!!secondary **Prop.** Local minima of convex $f$ is the global minima.

!!!

## Convex Optimization

A convex optimization program looks like the following:

$$
\begin{aligned}
    \min\quad& f_0(\bold x)\\
    \text{subject to }\quad & \left\{\begin{aligned}&
         \left.\begin{aligned}
        f_1(\bold x)&\leqslant 0\\
        f_2(\bold x)&\leqslant 0\\
        \qquad \vdots\\
        f_m(\bold x)&\leqslant 0
    \end{aligned}\right\}\text{Inequality Constraints}\\\\
    &\left.\begin{aligned}
        h_1(\bold x) &= 0\\
        \qquad \vdots\\
        h_p(\bold x) &= 0
    \end{aligned}\right\}\text{Equality Constraints}
    \end{aligned}
    \right.
\end{aligned}
$$

where $f_0\dots f_m: \R^m\to \R$ are convex, $h_1\dots h_p: \R^m\to \R$ are linear.

Let $X$ be the set of solutions / feasible region:

$$
X = \{\bold{x}\in\R^m\mid \bold{x} \text{ satisfies all the constraints} \}
$$

!!!warning **Remark.**

Linear functions of the form $\bold{a^\top x +b}$ are both convex and concave.

- LP is a special case of convex optimization

!!!

!!!secondary **Property.** Feasible region is convex

!!!

=== **Example.**

In $\R^2$, consider the constraints $f_1, f_2 \leqslant 0$ where:

$$
f_1(\bold x) = x_1^2 + x_2^2 - 1\\
f_2(\bold x) = e^{x_1} - x_2 - 1\\
$$

[!embed](https://www.desmos.com/calculator/sskqjyfkbl?embed)

We can see that the overlapped region is convex.

===

!!!secondary **Proposition.** Set of convex functions form a vector space
Let $f$ be convex

- Constant multiple: $af$ is convex
- Addition: $f + g$ is convex if $g$ is also convex
!!!

### Optimality Status

Infeasible
:   $X=\varnothing$, optimal value = $\infty$

Unbounded
:   $|X| = \infty$, optimal value = $-\infty$

Optimal
:   We either have a finite solution or the solution is infinite. For example $\min e^{-x}$, the optimal value is 0, but we need $x\to \infty$.

## Second Order Cone Programming (SOCP)

A general cone program has the form:

$$
\begin{aligned}
    \min\quad &\bold c^T\bold x\\
    \text{subject to}\quad & \|A_i\bold x + \bold b_i\|_2\leqslant \bold c_i^T\bold x +  d_i\qquad 1\leqslant i\leqslant m
\end{aligned}
$$

where $\|\cdot\|_2$ is the $l_2$ norm.

A *second order cone* has the following form:

$$
\{(\bold x,t)\in\R^2\times \R: \|\bold x\|_2 \leqslant t\}
$$

![](/assets/cone1.png){class="image-m"}

Linear programming is a special case of SOCP.

$$
\begin{matrix}
    \begin{aligned}
        \min \quad & \bold c^T \bold x\\
        \text{subject to}\quad &\begin{aligned}
            A\bold x &= \bold b\\
            \bold x &\geqslant 0
        \end{aligned}
    \end{aligned}

    &\iff
    &
    \begin{aligned}
        \min \quad & \bold c^T \bold x\\
        \text{subject to}\quad &\begin{aligned}
            \|A\bold x - \bold b\|_2 &\leqslant \bold 0^T\bold x + 0 = 0\\
            \|0\bold x + \bold 0\|_2 &\leqslant x_i
        \end{aligned}
    \end{aligned}
\end{matrix}
$$

## Semi-Definite Programming

!!!info **Def.** Semi-Definite Matrix

Matrix $A$ is semi-definite if 
- $a_{ij}\geqslant 0$
- $A^T = A$
- $\bold x^TA\bold x\geqslant 0$ for all $\bold x$ with non-negative entries

This is equivalent to saying all eigenvalues of $A$ is positive
!!!


!!!secondary **Proposition.** The set of semi-definite matrices is convex.
!!!


A semi-definite program has the following form:

$$
\begin{aligned}
    \min \quad&\bold c^T\bold x\\
    \text{subject to}\quad & \sum^n_{i=1}x_iA_i\geqslant B

\end{aligned}
$$

where $A_i$ is a matrix, $\bold x$ is a vector, $x_i$ is the $i$-th entry of $\bold x$

!!!info **Def.** Cone
A set $S\sub\R^n$ is a cone if $\forall\bold x\in S, \lambda \geqslant 0, \lambda\bold x\in S$.
- Non-negative multiples of $\bold x\in S$ is also in $S$.
!!!

## Lagrangian Dual Method

Now we wish to know the dual form of convex programs. For the general primal problem $(\text{P})$:

$$
(\text P)\qquad \begin{aligned}
    \min\quad & f_0(\bold x)\\
    \text{subject to}\quad & \left\{\begin{aligned}
        f_i(\bold x)&\leqslant 0 &&\quad i = 1\dots m\\
        h_j(\bold x) &= 0  &&\quad j = 1\dots p
    \end{aligned}\right.
\end{aligned}
$$

We need a variable for each $f_i$ and each $h_j$.


!!!info **Def.** Lagrangian

$$
{\cal L}(\bold x, \lambda, \mu) = f_0(\bold x) + \sum^m_{i=1}\lambda_if_i(\bold x) + \sum^p_{j=1}\mu_j h_j(\bold x)
$$
!!!

To satisfy *Thm. Weak Duality*, we need ${\cal L}(\bold x, \lambda, \mu)\leqslant f_0(\bold x)$.

If $\bold x$ is a solution to the program, then we know from the original constraints:

$$
\sum^p_{j=0}\mu_jh_j(\bold x) = 0\qquad \sum^m_{i=1}f_i(\bold x)\leqslant 0
$$

So there's no constraint on $\mu_j$, and we need $\lambda_i\geqslant 0$ for $\sum^m_{i=1}\lambda_if_i(\bold x)\leqslant 0$ to hold.

