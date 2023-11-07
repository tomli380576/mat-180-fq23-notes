---
icon: circle
---

# W4 - Network Flow Pt.2

## With discontinuous cost function

In the previous network flow model, the cost of an edge is linear:

$$
\text{Cost of arc }i\to j = c_{ij}x_{ij}
$$

Suppose now there’s a starting price $d_{ij}$ to send any amount of flow on arc $i\to j$. 

Let non-negative $x_{ij}$ be the flow of $i\to j$. 

We can model the initial cost with an indicator variable $y_{ij}$.

$$
y_{ij} = \begin{cases}
1 & \text{if need to pay to send flow on }i\to j\\
0 & \text{otherwise}

\end{cases}
$$

The total cost/objective function becomes:

$$
\min\sum_{(i\to j)\in A}c_{ij}x_{ij} + \sum_{(i\to j)\in A}d_{ij}y_{ij}
$$

idk what happened this week