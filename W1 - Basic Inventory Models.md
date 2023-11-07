# W1 - Basic Inventory Models


## Economic Order Quantity Model (EOQ)

### Scenario. Coffee Shop Inventory

A coffee shop needs to decide how much coffee beans to purchase and how frequently should they purchase. Factors to consider include:

- Beans could expire
- Storage cost
- Uncertainty in demand / supply. There could be a surge in customers
- Possible discounts for bulk purchase
- Contracts with bean providers (amortize fixed ordering)

#### Assumption

1. Everything is deterministic
2. Demand is constant (always the same amount of customers)
3. Only 1 product (1 type of coffee)
4. No backorders (coffee bean provider is always available)
5. Zero load time (supply instantly appears when ordered)
6. No discounts (coffee is always flat rate)

#### Input

| $d$ | Total annual demand (in this case pounds of coffee needed per year) |
| --- | ------------------------------------------------------------------- |
| $f$ | Fixed cost of an order (shipping & handling, etc.)                  |
| $c$ | Unit cost of each item (price per pound of coffee)                  |
| $h$ | Holding / Storage cost per item per year                            |

#### Decision Variables / Output

| $Q$ | Quantity / size of each order |
| --- | ----------------------------- |

#### Optimal Order Quantity $Q^*$

Since orders are fulfilled instantly, we only order a batch of coffee beans when stock goes to 0.

![Screenshot 2023-09-27 at 17.54.03.png](Screenshot_2023-09-27_at_17.54.03.png)

We can compute the following:

$$
\begin{aligned}
\text{Order Cost} &= f\cdot\overbrace{\frac dQ}^{\text{\# orders}} + \overbrace{cd}^\text{actual coffee cost}\\[10pt]
\text{Holding Cost} &= h\cdot\underbrace{\frac Q2}_{\small\substack{\text{avg.}\\\text{inventory size}}}
\end{aligned}
$$

Then the total cost $\text{TC}$ is:

$$
\text{TC}(Q) = f\frac dQ + cd + h\frac Q2
$$

Let the purchase quantity that minimizes total cost be $Q^*$:

$$
\begin{aligned}\frac{d}{dQ}(\text{TC}) &= -\frac{fd}{Q^2} + \frac h2 = 0\\[10pt]
Q^* &= \sqrt{\frac{2fd}{h}}
\end{aligned}
$$

September 29, 2023

The optimal total cost can be found by plugging in $Q^*$ into $\text{TC}$:

$$

\text{TC}(Q^*) = \sqrt{2fdh} + cd


$$

- **Derivation**
  $$
  \begin{aligned}
  \text{TC}(Q^*) &= f\frac{d}{Q^*}+cd + h\frac Q2\\
  &= \frac{fd}{\sqrt{\frac{2fd}{h}}} + cd + h\frac{\sqrt {\frac{2fd}{h}}}{2}\\
  &= fd\frac{\sqrt{\frac{2fd}{h}}}{\frac{2fd}{h}} + \frac h2 \sqrt{\frac{2fd}{h}} + cd\\
  &= h\sqrt{\frac{2fd}{h}} + cd\\
  &= \sqrt{2fdh} + cd
  \end{aligned}
  $$

We can find the optimum $Q^*$ when we graph the total cost at the intersection of holding cost $y(Q)=\frac {hQ}{2}$ and ordering cost $y(Q) = \frac{fd}{Q}$.

![Untitled](/assets/Untitled.png)

#### Sensitivity Analysis

Since $Q^*$ is likely not an integer, we are interested in what will happen if we pick a nearby integer.

![Screenshot 2023-09-29 at 17.59.10.png](/assets/Screenshot_2023-09-29_at_17.59.10.png)

In particular, we could find a range of $Q$ such that $\text{TC}(Q)$ is within some range around the optimal total cost.

=== [!badge variant="warning" text="Example 1"]

Find a range of $Q$ such that $\text{TC}$ is within 1% from the optimum.

$$
\text{TC}(Q)\leqslant (1 + 0.01)\cdot \text{TC}(Q^*)
$$

===

## The News Vendor Model

### Scenario. Selling Newspapers

A newspaper vendor needs to decide how many newspapers to buy from the news publisher. Newspapers expire in a day. Expired newspapers can be salvaged (returned to vendor) for a lower price.

#### Decision Variable

| Variable | Definition                                        |
| -------- | ------------------------------------------------- |
| $B$      | The number of newspapers to buy from the supplier |

#### Input & Objective Function

| Variable     | Definition                                                                   |
| ------------ | ---------------------------------------------------------------------------- |
| $d$          | Discrete R.V., the uncertain demand of newspapers                            |
| $P$ (unused) | The probability distribution of newspaper demand. $d\sim P$                  |
| $p$          | Sale price of each newspaper                                                 |
| $c$          | Cost of each newspaper                                                       |
| $s$          | Salvage price, sale price for expired newspapers. (e.g. recycling the paper) |

The 3 prices have the following inequality:

$$
p > c > s
$$

The total profit $\text{TP}$ is given by:

$$
\text{TP}(d, B) = \begin{cases}
(p-c)\cdot B & \text{if }d\geqslant B\\
(p-c)\cdot d + (s-c)\cdot (B-d) & \text{if }d < B
\end{cases}
$$

- We either sold all the newspapers or have to salvage some of them.

The objective is to maximize the expected value of $\text{TP}$ by picking the best $B$.

$$
\max_{B}\,\Bbb E[\text{TP}]
$$

October 2, 2023

Since $d$ is a discrete random variable, we have a probability mass function:

$$
\Bbb P(d=i) = p_i
$$

For simplicity assume that $i$ is finite, so $i=0,1,2\dots, M$ for some $M\in\N$.

Then the expectation is:

$$
\Bbb E[d] = \sum_ii\cdot \Bbb P(d=i)
$$

Fix $d=i$. If $i\geqslant B$, then:

$$
\text{TP}(B)=(p-c)\cdot B
$$

If $i < B$, then:

$$
\text{TP}(B)=(p-c)\cdot i + (s-c)(B-i)
$$

Combining the 2 cases we have the expected profit:

$$
\Bbb E[\text{TP}] =\sum^B_{i=0}p_i \big((p-c)i + (s-c)(B-i)\big) + \sum^M_{i=B+1}p_i(p-c)B
$$

### Optimum

For simpler notation let $f(B)$ be the expected profit $\Bbb E[\text{TP}]$ when we purchase $B$ newspapers.

Rearrange:

$$
\begin{aligned}
f(B) ={}& \sum^B_{i=0}p_i(pi + s(B-i) - cB)\\
&+\fcolorbox{green}{none}{$\displaystyle(1-\sum^B_{i=0}p_i)$}\cdot(p-c)\cdot B && \color{green} \small\text{See E.1}\\
={}&p\cdot \left[(\sum^B_{i=0}p_i i) + (1-\sum^B_{i=0}p_i)\cdot B\right] &&\small\blue{\Bbb E[\text{Sales}]}\\
&+s\sum^B_{i=0}p_i(B-i) &&\small\blue{\Bbb E[\text{Salvage}]}\\
&-cB&&\small\blue{\Bbb E[\text{Cost}]}

\end{aligned}
$$

=== [!badge variant="success" text="Explanation E.1"]

Since the profit when $i>B$ is fixed, we can pull it out of the sum:

$$
\begin{aligned}
\sum^M_{i=B+1}p_i(p-c)B &= (p-c)B\sum^M_{i=B+1}p_i\\ &= (p-c)B(1-\Bbb P(i \leqslant B))\\
&= (p-c)B(1-\sum^B_{i=0}p_i)
\end{aligned}
$$

===

Now we consider the finite difference of $f(B+1) - f(B)$.

$$
\begin{aligned}f(B+1)
={}&p\cdot \left[(\sum^{B+1}_{i=0}p_i \cdot i) + (1-\sum^{B+1}_{i=0}p_i)\cdot (B+1)\right]\\
&+s\sum^{B+1}_{i=0}p_i(B+1-i) \\
&-c(B+1)\\
f(B+1)-f(B) ={}&p\cdot(1-\sum^B_{i=0}p_i) + s\sum^B_{i=0}p_i - c\\
={}& (p-c)-(p-s)\cdot\fcolorbox{green}{none}{$\displaystyle\sum^B_{i=0}p_i$}

\end{aligned}
$$

Notice that we got the c.d.f. of $d$ to appear in the difference:

$$
Q(d) = \sum^d_{i=0}p_i
$$

Plug in $Q$:

$$
f(B+1) - f(B)=(p-c)-(p-s)Q(B)
$$

If the finite difference is positive, that means we are making more money by buying more newspapers. Otherwise we are losing money or not making more.

$$
\begin{aligned}
(p-c)-(p-s)\cdot Q(B) &\geqslant 0\\
\frac{p-c}{p-s}&\geqslant Q(B)\\\\
\hdashline\\
(p-c)-(p-s)\cdot Q(B) &< 0\\
\frac{p-c}{p-s}&< Q(B)
\end{aligned}
$$

#### [!badge variant="warning" text="Strategy."] Critical Ratio Method

This method requires $s<c<p$.

Let $\frac{p-c}{p-s}$ be the critical ratio. The smallest $B$ such that:

$$
Q(B)\geqslant\frac{p-c}{p-s}
$$

is the optimum number of newspapers to buy, $B^*$.

#### Sensitivity

Suppose the salvage price $s$ increases and all else equal.

$$
s\uparrow\implies p-s\darr \implies\frac{p-c}{p-s}\uarr\implies B^*\uarr
$$


## [!badge size="xl" text="Key Example."] Buying services

### Scenario. Signing a contract

Suppose we want to sign a contract with a lighting company to purchase their maintenance services. The amount of services to put on the contract will be fixed. But we don’t know how many services we will actually need.

- Contract too much: We get refunded for remaining services at a less price
- Contract too little: We need to pay more for the extra services on the fly

#### Decision Variable

| $B$ | The number of services to buy from the contracting company |
| --- | ---------------------------------------------------------- |

#### Input & Objective Function

| $D$    | A discrete random variable, the uncertain number of services we actually need |
| ------ | ----------------------------------------------------------------------------- |
| $c$    | Price of each service on the contract                                         |
| $s$    | Refund price                                                                  |
| $p$    | Price of extra services                                                       |
| $q(d)$ | PMF of $D$, same as $\Bbb P(D=d)$, in this case $d = 0,1,2,\dots$             |
| $Q(d)$ | CDF of $D$, same as $\Bbb P(D\leqslant d)$                                    |

The objective is to minimize the expected total cost $\text {TC}$ by picking the optimal $B$.

$$
\min_B\Bbb E[\text {TC}]
$$

The expression for $\Bbb E[{\text {TC}}]$ is:

$$
\begin{aligned}
\Bbb E[{\text {TC}}] = \underbrace{cB}_{\substack{\text{Contract}\\\text{Price}}} + \underbrace{p\sum_{d\geqslant B+1}q(d)\cdot (d-B)}_\text{Expected cost of extra services}-\underbrace{s\sum_{d < B}q(d)(B-d)}_\text{Expected total refund}
\end{aligned}
$$

Here we notice that $d-B$ and $B-d$ can be expressed as positive & negative parts.

$$
\Bbb E[\text{TC}] = cB + p\sum_{d\geqslant 0}q(d)(d-B)^+ + s\sum_{d\geqslant 0}q(d)(d-B)^-
$$

where $(d-B)^+ = \max(d-B, 0)$ and $(d-B)^- = \min(d-B, 0)$.

[!badge size="s" text="Strat. Critical Ratio Method"](#badge-variantwarning-textstrategy-critical-ratio-method) still works because $\Bbb E[\text{TC}]$ is dual of $\Bbb E[\text{Profit}]$ from the newsvenfor problem.

- Minimizing $\Bbb E[\text{TC}]$ is the same as maximizing $\Bbb E[\text{Profit}]$.
