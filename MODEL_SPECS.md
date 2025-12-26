[← Back to Project Overview](README.md)

# Mathematical Specification: Berlin Equilibrium Toy Model (2025)

## 0. Conventions

*   **Horizon:** One representative day, partitioned into $T$ time blocks.
*   **Units:** within-block objects are **per hour**; profits are **per day**.
*   **VAT Convention:**
    *   $p$: net-of-VAT price (€/unit).
    *   $\tau$: VAT rate.
    *   Consumer pays gross $p^g=(1+\tau)p$.
    *   $c_{mat}$: materials cost (€/unit net-of-VAT).
*   **Technology:** One bottleneck service station modeled as **M/G/1** within each block.

---

## I. Variable Declarations

### A. Indices and Time

*   $t \in \{1, \dots, T\}$: Time block index.
*   $h_t$: Length of block $t$ (hours).
*   $H \equiv \sum_{t=1}^T h_t$: Total open hours.

### B. Market Primitives

*   $\Lambda_t$: Potential arrival rate in block $t$ (customers/hour).
*   $\beta > 0$: Balking sensitivity (join probability $e^{-\beta W}$).

### C. Payment Composition

*   $a \in \{0,1\}$: Acceptance decision ($0=$ Cash-only, $1=$ Mixed).
*   $\phi \in [0,1]$: Card-mandatory fraction.
*   $\delta \in [0,1]$: Fraction of cash-capable customers preferring cards.

**Implied card share ($u$):**

$$
u(a) = \begin{cases} 
0 & \text{if } a=0 \\
\phi + (1-\phi)\delta & \text{if } a=1 
\end{cases}
$$

### D. Service Time Primitives

*   $S_c$: Cash service time (hours). Moments: $E[S_c], Var(S_c)$.
*   $S_k$: Card service time (hours). Moments: $E[S_k], Var(S_k)$.

### E. Capacity

*   $K_t$: Non-checkout capacity cap (kitchen/inventory) in block $t$.

### F. Prices and Costs

*   $p$: Net price (€).
*   $\tau$: VAT rate.
*   $c_{mat}$: Material marginal cost (€).
*   $\xi$: Fixed card cost (€/hour) if $a=1$.
*   $\psi$: Variable card fee rate on **gross** revenue.

### G. Labor

*   $w_{black}$: Informal wage (€/hour).
*   $w_{white}$: Formal wage (€/hour), where $w_{white} > w_{black}$.

### H. Enforcement Parameters

*   $\alpha \in [0,1]$: Declared fraction of cash sales (**Choice Variable**).
*   $\theta \ge 1$: Penalty multiplier on evaded VAT.
*   $\pi_{cash}, \pi_{card}$: Audit probabilities.
*   $d_{cash}$: Detection probability if cash-only.
*   $\kappa$: Sensitivity of detection to reporting gaps.
*   $\hat{u}$: Benchmark card share (defined endogenously below).

---

## II. Service Time Distribution

Per-customer service time $S$ is a mixture based on card share $u$:

$$ E[S(u)] = (1-u)E[S_c] + uE[S_k] $$

**Variance (Law of Total Variance):**

$$ Var(S(u)) = (1-u)Var(S_c) + uVar(S_k) + u(1-u)\big(E[S_c]-E[S_k]\big)^2 $$

**Second Moment:**

$$ E[S^2(u)] = Var(S(u)) + (E[S(u)])^2 $$

---

## III. Queueing Within Blocks

For block $t$ with served rate $Q_t$:

**Utilization:**

$$ \rho_t(Q_t, u) = Q_t \cdot E[S(u)] $$

**Expected Wait Time (Pollaczek–Khinchine):**

$$ W_t(Q_t, u) = E[S(u)] + \frac{Q_t E[S^2(u)]}{2(1-\rho_t(Q_t, u))} $$

---

## IV. Demand and Block Equilibrium

**1. Addressable Market:**

$$ \Lambda_{a,t}(a) = \begin{cases} \Lambda_t(1-\phi) & \text{if } a=0 \\ \Lambda_t & \text{if } a=1 \end{cases} $$

**2. Willing-to-Join Demand:**

$$ D_t(Q_t; a) = \Lambda_{a,t}(a) \exp\left(-\beta W_t(Q_t, u(a))\right) $$

**3. Block Equilibrium:**

The served rate $Q_t^*$ is the fixed point constrained by capacity $K_t$:

$$ Q_t^*(a) = \min \left\{ K_t, \; \Lambda_{a,t}(a) \exp\left(-\beta W_t(Q_t^*, u(a))\right) \right\} $$

---

## V. Day Aggregation

**Total Daily Units:**

$$ Q^{day}(a) = \sum_{t=1}^T h_t Q_t^*(a) $$

**Split Volumes:**

$$ Q_k^{day}(a) = u(a) Q^{day}(a) $$

$$ Q_c^{day}(a) = (1-u(a)) Q^{day}(a) $$

**Gross Revenues:**

$$ R_{cash}^g(a) = (1+\tau)p Q_c^{day}(a) $$

$$ R_{card}^g(a) = (1+\tau)p Q_k^{day}(a) $$

---

## VI. Labor Cost (Informality Wedge)

Labor is paid for $H$ hours. Informal wages ($w_{black}$) can only be financed via gross cash receipts.

**Informal Wage Share:**

$$ m(a) = \min \left\{ 1, \; \frac{R_{cash}^g(a)}{w_{black}H} \right\} $$

**Total Labor Cost:**

$$ C_{labor}(a) = m(a)w_{black}H + (1-m(a))w_{white}H $$

---

## VII. VAT Evasion

Declared units per day:

$$ Q_{decl}^{day}(a,\alpha) = Q_k^{day}(a) + \alpha Q_c^{day}(a) $$

Evaded VAT per day:

$$ VAT_{evaded}(a,\alpha) = \tau p (1-\alpha) Q_c^{day}(a) $$

---

## VIII. Enforcement: Data Pipeline & Endogenous Benchmark

### 1. Endogenous Benchmark ($\hat{u}$)

The authority's benchmark is a weighted average of an external anchor ($u_{ext}$) and a local inferred benchmark ($u_{loc}$).

$$ \hat{u} = \omega u_{ext} + (1-\omega)u_{loc} $$

**Local Benchmark ($u_{loc}$):**

Defined by the typical environment norms ($\bar{u}$ card usage and $\bar{\alpha}$ compliance):

$$ u_{loc} = \frac{\bar{u}}{\bar{u} + \bar{\alpha}(1-\bar{u})} $$

### 2. Underreporting Gap ($G$)

If $a=1$, the authority observes $Q_k^{day}$ and imputes total sales using $\hat{u}$.

$$ Q_{implied}^{day} = \frac{Q_k^{day}(a)}{\hat{u}} $$

$$ G(a,\alpha) = \max \left\{ 0, \; Q_{implied}^{day} - Q_{decl}^{day}(a,\alpha) \right\} $$

### 3. Audit Probability ($\pi_A$)

$$ \pi_A(a) = \begin{cases} \pi_{cash} & a=0 \\ \pi_{card} & a=1 \end{cases} $$

*(Typically $\pi_{card} \gg \pi_{cash}$)*

### 4. Detection Probability ($d$)

$$ d(a,\alpha) = \begin{cases} d_{cash} & a=0 \\ 1 - \exp(-\kappa G(a,\alpha)) & a=1 \end{cases} $$

### 5. Expected Penalty

$$ C_{audit}(a,\alpha) = \pi_A(a) \cdot d(a,\alpha) \cdot \theta \cdot VAT_{evaded}(a,\alpha) $$

---

## IX. Card Acceptance Costs

**Fixed Cost:**

$$ C_{card,fixed}(a) = a \cdot \xi \cdot H $$

**Variable Cost (on Gross Revenue):**

$$ C_{card,var}(a) = a \cdot \psi \cdot R_{card}^g(a) $$

**Total Card Cost:**

$$ C_{card}(a) = C_{card,fixed}(a) + C_{card,var}(a) $$

---

## X. Profit Optimization

**Operating Margin:** $(p - c_{mat})Q^{day}(a)$

**Full Profit Function:**

$$
\begin{aligned}
\Pi(a,\alpha) &= \underbrace{(p - c_{mat})Q^{day}}_{\text{Net Op. Profit}} \\
&\quad - C_{labor} - C_{card} \\
&\quad + \underbrace{\tau p (1-\alpha)Q_c^{day}}_{\text{Kept VAT}} \\
&\quad - C_{audit}
\end{aligned}
$$

**Optimization Sequence:**

1.  $\alpha^*(a) = \arg\max_{\alpha \in [0,1]} \Pi(a,\alpha)$
2.  $a^* = \arg\max_{a \in \{0,1\}} \Pi(a, \alpha^*(a))$