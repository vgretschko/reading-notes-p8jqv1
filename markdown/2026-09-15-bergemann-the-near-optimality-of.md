**The one thing to learn from this paper:** the profit a seller gives up by replacing the optimal nonlinear price schedule with a **two-part tariff — a fixed fee plus a constant markup over production cost —** is controlled by **one number**, the elasticity of marginal cost, m, and by nothing else. Set the markup equal to m, choose the fee optimally, and you are guaranteed at least m / (m+1)^(1 + 1/m) of the optimal profit **for every regular value distribution**. For monomial costs C(q) = q^k / k that guarantee is k^(-1/(k-1)), never below 1/e (about 37%), and **rising to 1 as costs get more convex**. Simplicity is nearly free exactly where two-part tariffs are actually used — cloud, utilities, telecoms, congestion-prone capacity — because those are the markets with steeply rising marginal cost.

## 1. What the researchers do

A monopolist sells a **divisible** good to a single buyer with private per-unit value v drawn from F, producing q units at a strictly increasing, strictly convex cost C(q). Since Mussa–Rosen (1978) we know the optimal mechanism: allocate x(v) maximizing x phi(v) - C(x) pointwise, where phi(v) = v - (1 - F(v)) / f(v) is the virtual value. The resulting price schedule depends on every detail of F and C, and can be arbitrarily complicated.

Firms do not do this. They post a **two-part tariff** (T, beta): pay T to join, then pay (1 + beta) C(q) for any quantity q you want. The paper asks the **approximation** question: for a given cost function, is there a pair (T, beta) — allowed to depend on F — such that pi(T, beta) >= rho pi*, for a constant rho that is **independent of F**?

The organizing primitive is the **elasticity of marginal cost**, eta(q) := q C''(q) / C'(q), which measures the percentage change in marginal cost per percentage change in quantity. For monomial costs it is constant and equals k - 1. The paper's assumptions are a lower bound eta >= m > 0 and, optionally, an upper bound eta <= M.

The paper then works through: an exactly-solved Pareto/monomial example; the general regular case; the non-regular case and menus; multiple product lines; and an extension to nonlinear buyer values.

## 2. Why the literature cares

Two reasons, and they point in different directions.

The first is **the simplicity-versus-optimality program** in mechanism design — Hartline–Roughgarden, Chawla et al., Cai et al., Babaioff et al. — which is almost entirely about **indivisible** goods: posted prices approximating optimal auctions, selling separately versus bundling. Nonlinear pricing with a genuinely divisible good and a real production cost has been nearly untouched since Wilson's 1993 monograph, which asked how a *finite menu* of quantity–payment pairs approximates the optimum. That restricts menu **cardinality**; this paper restricts the **functional form** instead, and lets the buyer choose freely from a continuum — orthogonal axes of simplicity, and the functional-form one is what firms actually use.

The second reason is that **this contract class is the procurement class**. A fixed fee plus a constant share of cost is exactly the cost-plus-fixed-fee / incentive-contract family of Laffont–Tirole (1993), McAfee–McMillan (1986) and Bajari–Tadelis (2001). It is also, remarkably, where the **prior-free** literature lands: Bergemann et al. (2023) show that the markup maximizing the worst-case ratio of profit to social surplus when demand is unknown equals the **elasticity of marginal cost** — the same markup beta = m that shows up here as the Bayesian approximation-optimal markup. Two completely different robustness criteria select the same rule. That coincidence is worth a paper on its own.

## 3. Why it is a contribution, and where it fits

This is the **first distribution-free approximation guarantee for nonlinear pricing with convex production costs**, and the structure of the answer is what makes it useful rather than merely new. The guarantee is:

- **distribution-free** — one constant works for all regular F, and computing it needs only the cost function, not an estimate of F;
- **monotone in the right primitive** — worse when costs are near-linear, better when they are convex, matching where the contract is actually observed;
- **tight**, with matching impossibility results on both the elasticity bound and the regularity assumption.

The proof strategy is the part worth stealing. The optimal profit is bounded above by a quantity that **splits into exactly two pieces matching the two instruments**:
pi*  <=  (1 - 1/k) * integral from v_0 to infinity of  f(v) [ W(v_0) + C(q(v)) ] dv,
where W(v) = max_q { v q - C(q) } and v_0 = inf{ v : phi(v) >= 0 }. The first term in the bracket is exactly what a **fixed fee** can collect; the second is exactly what a **markup** covers. One then only has to bound two ratios — q(v/(1+beta)) / q(v) and beta C(q(v/(1+beta))) / C(q(v)) — uniformly in v. **Lemma 4.2** does exactly that from the elasticity bounds: q(v/s) >= s^(-1/m) q(v) for s >= 1, and v q(v)/(M+1) <= C(q(v)) <= v q(v)/(m+1). The markup is functioning as a **contraction of the type space**, and the elasticity bound is precisely what controls how far that contraction moves the allocation.

## 4. The key theorem

**Theorem 1.** If eta(q) >= m for all q, then the *universal* markup beta := m admits, for every regular F, a fee T with
pi(T, beta)  >=  [ m / (m+1)^(1 + 1/m) ] * pi*,
improving to [ m / (m+1)^(1 + 1/m) ] * (M+1)/M if also eta <= M.

Note what "universal" is doing: the **markup does not depend on F at all**. Only the fee does. Specialized to C(q) = q^k / k (so m = M = k - 1) this reads pi(T, beta) >= k^(-1/(k-1)) pi*, which rises from 1/e as k -> 1 toward 1 as k -> infinity (**Proposition 4.1**), and that ratio is **tight** — for every epsilon there is a regular F on which no two-part tariff does better.

Three sharp complements:

- **Both instruments are needed.** With Pareto values and monomial costs, the two-part tariff with beta = 1/(lambda - 1) and T = ((k-1)/k) (1 + beta)^(-1/(k-1)) is **exactly optimal** (Proposition 3.1) — the virtual value is linear, so the markup reproduces phi exactly. But drop either piece and the approximation ratio becomes unbounded (Proposition 3.2). Pure markups and pure access fees both fail.
- **The elasticity bound is necessary.** With C(q) = e^q - 1, where eta(q) = q tends to 0 at the bottom, no two-part tariff gets a non-negligible fraction of the optimum (Proposition 4.2).
- **Regularity is necessary, and the fix is a small menu.** Beyond regularity, a single two-part tariff can be driven to an epsilon-fraction (Proposition 5.1) — one fixed fee cannot discriminate across types sitting in different ironing intervals. But **Theorem 2**: if the ironed virtual value has s ironing intervals, a menu of at most s + 1 entries sharing a **common markup** beta = m, differing only in fee and **capacity cap**, restores the same constant. The reading is attractive: *pay the same markup, and buy your way up to the next capacity tier with a larger fee.* And **Theorem 3** shows that of order s entries are unavoidable. So the required complexity scales with the **intrinsic non-regularity of demand**, not with the complexity of the optimal mechanism.

For **multiple product lines** with independent values — where optimal menus can be unboundedly large (Daskalakis et al. 2017) — **Theorem 4** gives pi* <= 4 SREV + 4 BREV, extending "sell separately or bundle" (Babaioff et al. 2020) from indivisible to divisible goods with costs, via the Cai et al. (2016) duality/flow framework. The bundling side turns out to be a **two-part tariff with zero markup**: pay one access fee, then buy everything **at production cost**.

## 5. Why this was hard and not done before

The approximation toolkit was built for indivisible goods, where the allocation is a probability in [0, 1] and "simple" means a posted price. A **convex production cost** breaks that in several places at once: revenue no longer equals virtual value times allocation, the benchmark pi* has no closed form, the Cai et al. duality framework needs reworking to carry a cost term, and even the *definition* of the bundling benchmark is unclear (a zero-markup access fee is not an obvious guess). The natural intuition is also wrong in an instructive way: as k -> infinity the monomial cost looks like unit supply, where Myerson makes posted pricing exactly optimal for **any** distribution, so one expects the non-regular case to get easy for large k. It does not — the worst-case distribution moves with k (Proposition 5.1). Finally, identifying **marginal-cost elasticity** as *the* sufficient statistic, rather than curvature or a bound on C'', is what makes a unit-free, distribution-free constant possible at all.

## 6. Where this would not do well

- **One buyer, one seller, no competition.** Rochet–Stole-style competitive nonlinear pricing is out of scope.
- **Worst-case guarantees are conservative.** 1/e is a floor over adversarial regular distributions; typical-case losses are presumably far smaller, and there is no average-case or empirical calibration.
- **The multi-product constant is loose.** A factor of 4 (8 across the two benchmarks) is qualitative, not a number a practitioner can act on.
- **Non-regularity is only handled through menu size.** If s is large, the "simple" mechanism is not simple, and nothing here tells you s without knowing F — which is precisely what the distribution-free framing was meant to avoid.
- **The general nonlinear-value case is open.** Multiplicatively separable values v U(q) reduce to the linear case by a change of variables (Proposition 7.1, with m replaced by a bound on (eta_C' - eta_U') / eta_U). For general supermodular u(v, q) there is only a **conjecture**, and the authors explain honestly why it is hard: the benchmark and the tariff guarantee are governed by opposite ends of the elasticity band, and an adversary can pull them apart.
- **No dynamics, no capacity constraints, no cost uncertainty**, and the seller is assumed to know F in order to set the fee even though the markup is universal.

## 7. How to cite / refer to this paper

Cite it as the paper that gives the **first distribution-free approximation guarantee for two-part tariffs in nonlinear pricing with convex costs**, with the guarantee **m / (m+1)^(1 + 1/m), governed solely by the elasticity of marginal cost**, tight, and with matching impossibility results for vanishing elasticity and for non-regular distributions.

> Bergemann, D., Y. Cai, J. Wu, and K. Zabarnyi (2026): "The Near-Optimality of Two-Part Tariffs for Nonlinear Pricing," Cowles Foundation Discussion Paper No. 2534.

Most useful as: (i) the **theoretical foundation for cost-plus-fixed-fee contracting** — the missing quantitative statement behind Laffont–Tirole and Bajari–Tadelis, and the natural citation when arguing that a procurement or utility tariff need not be optimal to be good enough; (ii) the source of the **"set the markup equal to the elasticity of marginal cost"** rule, striking precisely because Bergemann et al. (2023, 2025) reach it from a **prior-free** direction — cite the pair together to argue a rule is doubly robust; (iii) the **menu-size result**, an unusually concrete prescription that pricing complexity should scale with the number of ironing intervals in demand; and (iv) the **divisible-goods extension of "selling separately or bundling"**. For antecedents: Mussa–Rosen (1978) and Maskin–Riley (1984), Wilson (1993), and Cai et al. (2016).
