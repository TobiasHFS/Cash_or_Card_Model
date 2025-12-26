\[← Back to Project Overview](README.md)



\# Mathematical Specification: Berlin Equilibrium Toy Model (2025)



\## 0. Conventions



\*   \*\*Horizon:\*\* One representative day, partitioned into $T$ time blocks.

\*   \*\*Units:\*\* within-block objects are \*\*per hour\*\*; profits are \*\*per day\*\*.

\*   \*\*VAT Convention:\*\*

&nbsp;   \*   $p$: net-of-VAT price (€/unit).

&nbsp;   \*   $\\tau$: VAT rate.

&nbsp;   \*   Consumer pays gross $p^g=(1+\\tau)p$.

&nbsp;   \*   $c\_{mat}$: materials cost (€/unit net-of-VAT).

\*   \*\*Technology:\*\* One bottleneck service station modeled as \*\*M/G/1\*\* within each block.



---



\## I. Variable Declarations



\### A. Indices and Time

\*   $t \\in \\{1,\\dots,T\\}$: Time block index.

\*   $h\_t$: Length of block $t$ (hours).

\*   $H \\equiv \\sum\_{t=1}^T h\_t$: Total open hours.



\### B. Market Primitives

\*   $\\Lambda\_t$: Potential arrival rate in block $t$ (customers/hour).

\*   $\\beta > 0$: Balking sensitivity (join probability $e^{-\\beta W}$).



\### C. Payment Composition

\*   $a \\in \\{0,1\\}$: Acceptance decision ($0=$ Cash-only, $1=$ Mixed).

\*   $\\phi \\in \[0,1]$: Card-mandatory fraction.

\*   $\\delta \\in \[0,1]$: Fraction of cash-capable customers preferring cards.



\*\*Implied card share ($u$):\*\*

$$

u(a) = \\begin{cases} 

0 \& \\text{if } a=0 \\\\

\\phi + (1-\\phi)\\delta \& \\text{if } a=1 

\\end{cases}

$$



\### D. Service Time Primitives

\*   $S\_c$: Cash service time (hours). Moments: $E\[S\_c], Var(S\_c)$.

\*   $S\_k$: Card service time (hours). Moments: $E\[S\_k], Var(S\_k)$.



\### E. Capacity

\*   $K\_t$: Non-checkout capacity cap (kitchen/inventory) in block $t$.



\### F. Prices and Costs

\*   $p$: Net price (€).

\*   $\\tau$: VAT rate.

\*   $c\_{mat}$: Material marginal cost (€).

\*   $\\xi$: Fixed card cost (€/hour) if $a=1$.

\*   $\\psi$: Variable card fee rate on \*\*gross\*\* revenue.



\### G. Labor

\*   $w\_{black}$: Informal wage (€/hour).

\*   $w\_{white}$: Formal wage (€/hour), where $w\_{white} > w\_{black}$.



\### H. Enforcement Parameters

\*   $\\alpha \\in \[0,1]$: Declared fraction of cash sales (\*\*Choice Variable\*\*).

\*   $\\theta \\ge 1$: Penalty multiplier on evaded VAT.

\*   $\\pi\_{cash}, \\pi\_{card}$: Audit probabilities.

\*   $d\_{cash}$: Detection probability if cash-only.

\*   $\\kappa$: Sensitivity of detection to reporting gaps.

\*   $\\hat{u}$: Benchmark card share (defined endogenously below).



---



\## II. Service Time Distribution



Per-customer service time $S$ is a mixture based on card share $u$:



$$ E\[S(u)] = (1-u)E\[S\_c] + uE\[S\_k] $$



\*\*Variance (Law of Total Variance):\*\*

$$ Var(S(u)) = (1-u)Var(S\_c) + uVar(S\_k) + u(1-u)\\big(E\[S\_c]-E\[S\_k]\\big)^2 $$



\*\*Second Moment:\*\*

$$ E\[S^2(u)] = Var(S(u)) + (E\[S(u)])^2 $$



---



\## III. Queueing Within Blocks



For block $t$ with served rate $Q\_t$:



\*\*Utilization:\*\*

$$ \\rho\_t(Q\_t, u) = Q\_t \\cdot E\[S(u)] $$



\*\*Expected Wait Time (Pollaczek–Khinchine):\*\*

$$ W\_t(Q\_t, u) = E\[S(u)] + \\frac{Q\_t E\[S^2(u)]}{2(1-\\rho\_t(Q\_t, u))} $$



---



\## IV. Demand and Block Equilibrium



\*\*1. Addressable Market:\*\*

$$ \\Lambda\_{a,t}(a) = \\begin{cases} \\Lambda\_t(1-\\phi) \& \\text{if } a=0 \\\\ \\Lambda\_t \& \\text{if } a=1 \\end{cases} $$



\*\*2. Willing-to-Join Demand:\*\*

$$ D\_t(Q\_t; a) = \\Lambda\_{a,t}(a) \\exp\\left(-\\beta W\_t(Q\_t, u(a))\\right) $$



\*\*3. Block Equilibrium:\*\*

The served rate $Q\_t^\*$ is the fixed point constrained by capacity $K\_t$:

$$ Q\_t^\*(a) = \\min \\left\\{ K\_t, \\; \\Lambda\_{a,t}(a) \\exp\\left(-\\beta W\_t(Q\_t^\*, u(a))\\right) \\right\\} $$



---



\## V. Day Aggregation



\*\*Total Daily Units:\*\*

$$ Q^{day}(a) = \\sum\_{t=1}^T h\_t Q\_t^\*(a) $$



\*\*Split Volumes:\*\*

$$ Q\_k^{day}(a) = u(a) Q^{day}(a) $$

$$ Q\_c^{day}(a) = (1-u(a)) Q^{day}(a) $$



\*\*Gross Revenues:\*\*

$$ R\_{cash}^g(a) = (1+\\tau)p Q\_c^{day}(a) $$

$$ R\_{card}^g(a) = (1+\\tau)p Q\_k^{day}(a) $$



---



\## VI. Labor Cost (Informality Wedge)



Labor is paid for $H$ hours. Informal wages ($w\_{black}$) can only be financed via gross cash receipts.



\*\*Informal Wage Share:\*\*

$$ m(a) = \\min \\left\\{ 1, \\; \\frac{R\_{cash}^g(a)}{w\_{black}H} \\right\\} $$



\*\*Total Labor Cost:\*\*

$$ C\_{labor}(a) = m(a)w\_{black}H + (1-m(a))w\_{white}H $$



---



\## VII. VAT Evasion



Declared units per day:

$$ Q\_{decl}^{day}(a,\\alpha) = Q\_k^{day}(a) + \\alpha Q\_c^{day}(a) $$



Evaded VAT per day:

$$ VAT\_{evaded}(a,\\alpha) = \\tau p (1-\\alpha) Q\_c^{day}(a) $$



---



\## VIII. Enforcement: Data Pipeline \& Endogenous Benchmark



\### 1. Endogenous Benchmark ($\\hat{u}$)

The authority's benchmark is a weighted average of an external anchor ($u\_{ext}$) and a local inferred benchmark ($u\_{loc}$).



$$ \\hat{u} = \\omega u\_{ext} + (1-\\omega)u\_{loc} $$



\*\*Local Benchmark ($u\_{loc}$):\*\*

Defined by the typical environment norms ($\\bar{u}$ card usage and $\\bar{\\alpha}$ compliance):

$$ u\_{loc} = \\frac{\\bar{u}}{\\bar{u} + \\bar{\\alpha}(1-\\bar{u})} $$



\### 2. Underreporting Gap ($G$)

If $a=1$, the authority observes $Q\_k^{day}$ and imputes total sales using $\\hat{u}$.



$$ Q\_{implied}^{day} = \\frac{Q\_k^{day}(a)}{\\hat{u}} $$

$$ G(a,\\alpha) = \\max \\left\\{ 0, \\; Q\_{implied}^{day} - Q\_{decl}^{day}(a,\\alpha) \\right\\} $$



\### 3. Audit Probability ($\\pi\_A$)

$$ \\pi\_A(a) = \\begin{cases} \\pi\_{cash} \& a=0 \\\\ \\pi\_{card} \& a=1 \\end{cases} $$

\*(Typically $\\pi\_{card} \\gg \\pi\_{cash}$)\*



\### 4. Detection Probability ($d$)

$$ d(a,\\alpha) = \\begin{cases} d\_{cash} \& a=0 \\\\ 1 - \\exp(-\\kappa G(a,\\alpha)) \& a=1 \\end{cases} $$



\### 5. Expected Penalty

$$ C\_{audit}(a,\\alpha) = \\pi\_A(a) \\cdot d(a,\\alpha) \\cdot \\theta \\cdot VAT\_{evaded}(a,\\alpha) $$



---



\## IX. Card Acceptance Costs



\*\*Fixed Cost:\*\*

$$ C\_{card,fixed}(a) = a \\cdot \\xi \\cdot H $$



\*\*Variable Cost (on Gross Revenue):\*\*

$$ C\_{card,var}(a) = a \\cdot \\psi \\cdot R\_{card}^g(a) $$



\*\*Total Card Cost:\*\*

$$ C\_{card}(a) = C\_{card,fixed}(a) + C\_{card,var}(a) $$



---



\## X. Profit Optimization



\*\*Operating Margin:\*\* $(p - c\_{mat})Q^{day}(a)$



\*\*Full Profit Function:\*\*

$$ \\Pi(a,\\alpha) = (p - c\_{mat})Q^{day}(a) - C\_{labor}(a) - C\_{card}(a) - \\underbrace{VAT\_{evaded}}\_{\\text{Assumed retained}} - C\_{audit}(a,\\alpha) $$



\*Note: In this accounting, $VAT\_{evaded}$ is subtracted from the "costs" (or added to revenue) effectively. Standard accounting profit is Net Revenue - Costs. Here, we model it as:\*



$$ 

\\Pi(a,\\alpha) = \\underbrace{(p - c\_{mat})Q^{day}}\_{\\text{Net Op. Profit}} 

\- C\_{labor} - C\_{card} 

\+ \\underbrace{\\tau p (1-\\alpha)Q\_c^{day}}\_{\\text{Kept VAT}} 

\- C\_{audit} 

$$



\*\*Optimization Sequence:\*\*

1\.  $\\alpha^\*(a) = \\arg\\max\_{\\alpha \\in \[0,1]} \\Pi(a,\\alpha)$

2\.  $a^\* = \\arg\\max\_{a \\in \\{0,1\\}} \\Pi(a, \\alpha^\*(a))$

