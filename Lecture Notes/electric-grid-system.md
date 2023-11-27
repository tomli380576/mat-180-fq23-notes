---
icon: circle
---

# Electric Grid System

## Complex Numbers Review

These are links to LibreTexts.

- [Properties of Conjugate](<https://math.libretexts.org/Courses/Lake_Tahoe_Community_College/A_First_Course_in_Linear_Algebra_(Kuttler)/06%3A_Complex_Numbers/6.01%3A_Complex_Numbers#Theorem_.5C(.5CPageIndex.7B3.7D.5C):_Properties_of_the_Conjugate-14531>)
- [Inverses](<https://math.libretexts.org/Courses/Lake_Tahoe_Community_College/A_First_Course_in_Linear_Algebra_(Kuttler)/06%3A_Complex_Numbers/6.01%3A_Complex_Numbers#Definition_.5C(.5CPageIndex.7B2.7D.5C):_Inverse_of_a_Complex_Number-14531>)

:::thm

#### **Thm.** Euler's Equation

$$
e^{i\pi} + 1 = 0
$$

:::

:::cor

#### **Cor.** Euler's Identity

Let $m, \theta\in\R$

$$
\begin{aligned}
    e^{i\theta} &= \cos \theta + i \sin\theta
\end{aligned}
$$

If $z_1 = m_1e^{i\theta_1}, z_2 = m_2e^{i\theta_2}$,

$$
z_1z_2 = m_1m_2e^{i(\theta_1 + \theta_2)}
$$

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
         &= \text{Re}(me^{i\omega t + \phi})\\
         &= \text{Re}(me^{i\phi}e^{i\omega t})
\end{aligned}
$$

### Current

For current, define

$$
c(t) = m\cdot\cos(\omega t + \theta)
$$


