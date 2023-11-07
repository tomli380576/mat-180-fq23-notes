# W2 - Linear Programming

We will use $\bold x$ to indicate a vector, $x_i$ to indicate the element of $\bold x$. The $\leqslant, \geqslant$ comparators are element–wise comparison. $\bold 0$ is the zero vector.

October 6, 2023 

## Linear Programming

### Scenario. Production Problem

Consider a factory that has a limited amount of resources and produces a finite number of products.

#### Input

| $m$ | Total number of kinds of resources |
| --- | --- |
| $n$ | Total number of products |
| $b_i$ | Amount we have for resource $i$ |
| $c_j$ | Unit profit of product $j$ |
| $a_{ij}$ | Amount of resource $i$ needed to make 1 unit of product $j$ |

#### Decision Variable

| $x_j$ | Number of product $j$ to make, $1\leqslant j\leqslant n$, non negative |
| --- | --- |

#### Objective

We want to maximize the total profit, which is the sum of profits over all products:

$$
\max \sum_{1\leqslant j\leqslant n}c_jx_j
$$

#### Constraints

For each resource $i$, we can’t use more than what we have:

$$
\sum_{1\leqslant j\leqslant n}a_{ij}x_j\leqslant b_i\hspace 2em1\leqslant i\leqslant m
$$

We shouldn’t make negative number of products either:

$$
x_j\geqslant 0, \forall j
$$

#### Matrix-Vector Form

$$
\begin{aligned}
\max &&& c^T\bold x\\
\text{subject to}&&&\begin{aligned}
A\bold x &\leqslant \bold b\\
\bold x&\geqslant \bold 0
\end{aligned}
\end{aligned}
$$

Where $c, x$ are vectors in $\R^m$:

$$
\bold c=\begin{bmatrix}
c_1\\c_2\\\vdots\\c_m
\end{bmatrix} 
\hspace1em
\bold x=\begin{bmatrix}
x_1\\x_2\\\vdots\\x_m
\end{bmatrix}
$$

and $A$ is a $m\times n$ matrix:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}\\
\vdots & & \ddots & \vdots\\
a_{m1} & a_{m2} & \cdots & a_{mn}

\end{bmatrix}
$$

#### EX.1 2D Example

The feasible region for the linear program:

$$
\begin{aligned}
\max &&& 8x_1 + 5x_2\\
\text{subject to}&&&\left\{\begin{aligned}
2x_1 + x_2 & \leqslant 6\\
2x_1 + 2x_2 & \leqslant 8\\
x_1, x_2 & \geqslant 0\\
\end{aligned}\right.

\end{aligned}
$$

![Screenshot 2023-10-06 at 17.45.31.png](Screenshot_2023-10-06_at_17.45.31.png)

#### Def. Standard Form

$$
\begin{aligned}
\max &&& \bold c^T\bold x\\
\text{subject to}&&&\begin{aligned}
A\bold x &\leqslant \bold b\\
\bold x&\geqslant \bold 0
\end{aligned}
\end{aligned}
$$

### Duality - Resource Buyer

Consider the resource buyer perspective. For each resource $i$, we have a bid price $y_i$.

#### Objective

Minimize total cost.

$$
\min \sum_{1\leqslant i \leqslant m} b_i y_i
$$

#### Constraint

For each product $j$, the price to sell all the resources needed should be greater than the profit of $j$.

$$
\sum_{1\leqslant i\leqslant m} a_{ij} y_i\geqslant c_j
$$

#### Matrix-Vector Form

$$
\begin{aligned}
\max &&& \bold b^T\bold y\\
\text{subject to}&&&\begin{aligned}
A^T\bold y &\geqslant \bold c\\
\bold y&\geqslant \bold 0
\end{aligned}
\end{aligned}
$$

This is the Dual problem of the original [Primal problem](W2%20-%20Linear%20Programming%20b280afc09ed04fc0bbed484b56f62e21.md)

#### Thm. Weak & Strong Duality

For each feasible solution $\bold x$ of the primal problem, and each feasible solution $\bold y$ of the dual problem, we have the following:

1. Weak Duality.
    
    $$
    \bold c^T\bold x\leqslant \bold b^T\bold y
    $$
    
2. Strong Duality (all linear programs have strong duality if optimal solutions $\bold x^*, \bold  y^*$ exists for primal and dual)
    
    $$
    \bold c^T\bold x^* = \bold b^T\bold y^*
    $$
    

October 9, 2023 

$\diamond\;\textbf{Proof.}$  (Weak Duality) 

Let $\bold x, \bold y$ be feasible solutions for the primal and dual problem respectively. 

Then by definition of feasible, we have

$$
A\bold x \leqslant \bold b\hspace2em \bold x\geqslant 0\hspace2em A^T \bold y \geqslant \bold c\hspace2em \bold y\geqslant 0
$$

We need to show $\bold c^T\bold x\leqslant \bold b^T\bold y$:

$$
\bold c^T\bold x\leqslant {\underbrace{(A^T\bold y)}_{\text{bound }\bold c}}^T\bold x = \bold y^TA\bold x\leqslant \bold y^T\underbrace{\bold b}_{\text{bound }A\bold x} = \bold b^T\bold y
$$

#### Thm. Complementary Slackness

![Screenshot 2022-11-08 at 2.07.01 PM.png](Screenshot_2022-11-08_at_2.07.01_PM.png)

The slack variables are just a vector that turns $\leqslant, \geqslant$ in the constraints into equality:

$$
\begin{aligned}
\bold w &= \bold b - A\bold x\\
\bold z &=  A^T\bold y - \bold c\\
\end{aligned}
$$

- Basically primal solution times dual slack is 0, dual solution times primal slack is also 0 for linear programs. The optimal objective $\zeta$ and $\xi$ are equal.

### Optimization Output / Infer Dual Results from Primal

A linear program can have the following statuses:

1. Finite, we have a finite optimal solution
2. Unbounded, the objective can go to $\pm\infty$
3. Infeasible, no solution exists that satisfies all the constraints

By strong duality, the results of Primal & Dual are linked:

| Primal | Dual |
| --- | --- |
| Optimal | Optimal |
| Unbounded | Infeasible |
| Infeasible | Unbounded or Infeasible |

### Sensitivity

Suppose we change one of the constraints $b_i$ to $b_i + \Delta b_i$.

Let $\zeta$ be the value of the objective function.

1. If $\Delta b_i > 0$, then the new optimal $\zeta_{new}$ should be at least the original solution because we loosened the constraint.
    
    $$
    \zeta_{new} \geqslant\zeta_{old}
    $$
    
2. If $\Delta b_i  < 0$, the original $\bold x^*$ might not be in the new feasible region. If there’s another solution, it should be at most the original because we tightened the constraint.
    
    $$
    \zeta_{new}\leqslant \zeta _{old}\hspace2em{\text{if feasible}}
    $$
    

---

Go to next:

[W3 - Network Flow Pt.1](W3%20-%20Network%20Flow%20Pt%201%2067d861e40761479f9d21e4dc59ff840e.md)