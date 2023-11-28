---
icon: circle
---

# Electric Grid System / Optimal Power Flow

!!!warning Notation

For this class, we use $z^*$ to denote the conjugate of $z\in\Complex$. We will use $\text i$ or $\text j$ (upright i and j) for the complex number $\sqrt {-1}$ to differentiate from the regular variables $i,j$.

!!!

## Complex Numbers Review

These are links to LibreTexts. (They use $\overline z$ for conjugate)

- [Properties of Conjugate](<https://math.libretexts.org/Courses/Lake_Tahoe_Community_College/A_First_Course_in_Linear_Algebra_(Kuttler)/06%3A_Complex_Numbers/6.01%3A_Complex_Numbers#Theorem_.5C(.5CPageIndex.7B3.7D.5C):_Properties_of_the_Conjugate-14531>)
- [Inverses](<https://math.libretexts.org/Courses/Lake_Tahoe_Community_College/A_First_Course_in_Linear_Algebra_(Kuttler)/06%3A_Complex_Numbers/6.01%3A_Complex_Numbers#Definition_.5C(.5CPageIndex.7B2.7D.5C):_Inverse_of_a_Complex_Number-14531>)

:::thm

#### **Thm.** Euler's Equation

$$
e^{\text i\pi} + 1 = 0
$$

<p></p>
:::

:::cor

#### **Cor.** Euler's Identity

Let $m, \theta\in\R$

$$
\begin{aligned}
    e^{\text i\theta} &= \cos \theta + \text i \sin\theta
\end{aligned}
$$

If $z_1 = m_1e^{\text i\theta_1}, z_2 = m_2e^{\text i\theta_2}$,

$$
z_1z_2 = m_1m_2e^{\text i(\theta_1 + \theta_2)}
$$

<p></p>
:::

## Physics Setup

In real electrical grids we use alternating current, so voltage $v$ and current $c$ changes with time $t$.

### Voltage

Let $v(t)$ be the voltage at time $t$, define:

$$
v(t) = m\cdot \cos(\omega t + \phi)
$$

where $m$ is the magnitude, $\omega$ is the angular frequency, and $\phi$ represents phase shift. All of which are constants in $\R$.

We can get rid of the cosine function by taking the real part of $v(t)$ in the complex plane.

$$
\begin{aligned}
    v(t) &= m\cdot \cos(\omega t + \phi) \\
         &= \text{Re}(me^{\text i\omega t + \phi})\\
         &= \text{Re}(me^{\text i\phi}e^{\text i\omega t})
\end{aligned}
$$

### Current

For current, define

$$
c(t) = m\cdot\cos(\omega t + \theta)
$$

### Kirchhoff's Laws

For an undirected network $G = (N, E)$, suppose we have $|N| = n$ nodes.

Voltage Law:

$$
\sum_{i} V_i = 0
$$

Current Law:

$$
I_i^g - I_i^d = \sum_{ij\in E} I_{ij}
$$

where $I_i^g$ is the generated current at node $i$, $I_i^d$ is the demanded current at node $i$.

By Ohm's Law, current is voltage over resistance:

$$
I_{ij} = Y_{ij}(V_i - V_j)
$$

where $Y_{ij}$ acts as the inverse of the resistance($Z_{ij}$) term for the edge $ij$. (Line admittance [wikipedia](https://en.wikipedia.org/wiki/Admittance)). This is a known input for the problem.

$$
Y = g + \text ib
$$

for some constant $g, b\in \R$. ($g$ is conductance, $b$ is susceptance).

AC Power on edge $ij$ is voltage times current's complex conjugate:

$$
S_{ij} = V_iI^*_{ij}\\
S_{ij}\in\Complex
$$

and the power flow on edge $ij$ is:

$$
\begin{aligned}
S_i^g - S_i^d &= \sum_{j\in N} S_{ij} \qquad \forall i\in N\\
S_{ij} &= V_iI^*_{ij}\\
&= V_i\cdot (Y_{ij}(V_i - V_j))^*\\
&= Y_{ij}^*V_iV_i^* - Y_{ij}^*V_iV_j^*\\
&= Y_{ij}^*(|V_i|^2 - V_iV_j^*)
\end{aligned}
$$

The total power transmitted from node $i$ is the sum of $S_{ij}$ over all nodes $j$ that are connected to $i$.

$$
\begin{aligned}
    S_i &= \sum_{\{j\,\mid\, ij\text{ is connected}\}} S_{ij}\\
    &= \sum_{\{j\,\mid\, ij\text{ is connected}\}}Y_{ij}^*(|V_i|^2 - V_iV_j^*)
\end{aligned}
$$

This is known as the [Bus Injection Model](https://invenia.github.io/blog/2020/12/04/pf-intro/#bus-injection-model).

## Constraints and Objective

### Side Constraints

- Generator limits (can only produce power in a range)

  $$
    S_i^{g, l}\leqslant S_i^g\leqslant S_i^{g, u}
  $$

  for some constants $S_i^{g, l}, S_i^{g, u}\in\R$

- Line thermal limit (can't carry too much power over each edge)

  $$
    |S_{ij}|\leqslant S_{ij}^u
  $$

- Bus voltage limit (can't produce too much voltage at 1 node)

  $$
    V_i^l \leqslant |V_i|\leqslant V_i^u
  $$

- Phase angle difference (not too much phase shift)

  $$
    |\theta_i - \theta_j|\leqslant \theta_{ij}^\Delta
  $$

### Optimal Power Flow

Let $S_i = p_i + \text i q_i$. The objective is to minimize the total cost:

$$
\min \sum_{i\in N} c_{i,2}p_{i,g}^2 + c_{i,1}p_{i,g} + c_{i,0}
$$
