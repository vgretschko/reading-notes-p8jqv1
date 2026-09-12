**The one thing to learn from this paper:** a buyer who knows nothing about the supplier's cost
function — no prior, no moments, no support — can still guarantee themselves a constant fraction of
the first-best social surplus, and the mechanism that does it is the crudest one imaginable: pay the
seller a fixed share of your own gross utility, `t(q) = z·u(q)`. No menu, no screening, no
distributional input. The optimal share is essentially your own demand elasticity parameter, and
**no mechanism of any complexity does better in the worst case**.

## 1. What the researchers do

A buyer procures quantity (or quality) `q` from a seller with privately known cost function `c(q)`,
assumed increasing and convex with `c(0) = 0`. The buyer's gross utility is isoelastic,
`u(q) = q^σ/σ` with `σ ∈ (0,1)`, so demand elasticity is `1/(1−σ)`. Crucially, the buyer has **no
prior** over `c` — only the knowledge that `c` lies in some class `C`.

Without a prior, expected surplus is undefined, so the authors evaluate a mechanism by its **buyer
surplus ratio guarantee**: `min over c in C of U(c,t)/W(c)`, where `W(c)` is the full-information
efficient social surplus. The **competitive ratio** is the best such guarantee over all mechanisms.
This is the standard worst-case criterion from theoretical computer science, imported into a
Baron–Myerson-style screening problem.

The result is that the maximiser is a **constant utility share mechanism**: the buyer commits to
handing over a fixed fraction `z` of gross utility, whatever quantity arrives. The seller solves
`max_q {z·u(q) − c(q)}` and never needs to know how the transfer decomposes into `z` and `u`.

## 2. Why does the literature care about this?

Robust mechanism design has a persistent embarrassment: its guarantees tend to degrade to nothing
as the environment grows. In the canonical prior-free pricing problem with values supported on
`[1,h]`, Hartline and Roughgarden establish a competitive ratio of `1/(1+ln h)`, which vanishes as
the support widens. Neeman's bound similarly collapses once you stop parameterising by the ratio of
expected to maximal value. The practical reading has been that prior-free design buys robustness at
the cost of any usable guarantee.

This paper breaks that pattern. Over the class of **all** convex cost functions — an enormous
adversary set, unbounded above and below — the guarantee is a strictly positive constant depending
on one number, `σ`. Convexity of cost (equivalently, concavity of demand) is doing the work: it
rules out the flat-marginal-cost configurations that drive the pricing bounds to zero.

The second reason the literature cares is the **simple-versus-optimal** question. Rogerson (2003)
showed a binary fixed-price/cost-reimbursement menu attains 3/4 of the optimum under a specific
parameterisation; Chu and Sappington (2007) obtained `2/e` across a wider class. Both compare simple
rules to a Bayesian optimum under assumed distributions. Here the comparison is to the
full-information first best, and holds for every cost function.

## 3. Why is it a contribution and where does it fit?

It sits at the junction of Baron–Myerson regulation, the simple-contracts literature, and the
competitive-ratio tradition, and it converts a computer-science criterion into a genuinely economic
statement: the optimal robust rule is indexed by **demand elasticity alone**.

Three features make it more than a bound. First, **uniqueness** — the constant share rule is not
merely one optimal mechanism but the only one; any `t` with a non-constant share `t(q)/u(q)` is
strictly beaten at some cost realisation. Second, a **Bayesian foundation**: the authors exhibit a
power distribution over marginal costs whose Bayes-optimal mechanism converges to the constant share
rule, establishing a saddle point and so closing the usual maximin–minimax gap. Third, the result
extends to **regulation** (Theorem 4: a regulator weighting profit by `α` uses share `zα*`, with the
ratio rising to 1 as `α → 1`) and, by duality, to **nonlinear pricing** à la Mussa–Rosen.

The paper also offers a rationalisation of practice: cost-plus contracts and markup pricing, usually
treated as naive, emerge as optimal responses to distributional ignorance.

## 4. What is a key figure or theorem from the paper?

**Theorem 2 (Buyer Surplus Guarantee).** For every mechanism `t`, `inf over c in Ccx of
U(c,t)/W(c) ≤ B(σ)`, with equality **if and only if** `t(q) = z*(σ)·u(q)`.

Two numerical facts make it memorable. The optimal share tracks the utility exponent almost exactly,
`|z*(σ) − σ| ≤ 0.12`, and the guarantee is near-linear in it, `|B(σ) − (1−σ)| ≤ 0.16`. In the linear
cost case the guarantee is `σ^(σ/(1−σ))`, bounded below by `1/e ≈ 0.37`, with buyer and seller
together capturing at least `2/e ≈ 0.74` of efficient surplus.

The sharpest result for market designers is the multi-seller extension: a VCG-style implementation of
the same aggregate schedule gives `CRn = CR1 = B(σ)` for every `n ≥ 1`. **Competition does not
improve the worst-case guarantee at all** — it helps for particular cost profiles, never in the worst
case.

## 5. Why was this hard and not done before?

The obstacle is the adversary's richness. With linear costs the worst case is a one-dimensional
search. Over all convex functions, the whole function enters the denominator through `W(c)`, not just
marginal cost at the supplied quantity, so one cannot optimise pointwise. The authors' move is to
show the binding cost functions are **piecewise linear with a single kink**: zero cost up to the
supplied quantity `q̂`, then constant marginal cost `t'(q̂)`. That reduces an infinite-dimensional
minimisation to two numbers. Making the first-order condition globally rather than locally optimal
requires a concavification argument (Lemma 1) for non-concave transfer schedules.

Theorem 3 then dispenses with isoelastic utility entirely by measuring cost curvature **in utils**:
`δ = u·c''(u)/c'(u)`, with `σ = 1/(1+δ)`. The elasticity decomposition shows why no separate
convexity assumption is needed — rising marginal cost and diminishing marginal value are perfect
substitutes in generating the curvature the bound depends on.

## 6. Where would this not do well?

**The guarantee dies exactly where procurement is largest.** `B(σ) → 0` as `σ → 1`, the elastic,
near-linear-utility case — which describes commodity procurement at scale. The comforting `1/e` floor holds only for the
linear cost case, not for the convex class the main theorem covers.

**It demands the buyer knows `σ` precisely.** The paper sells parsimony — a procurement officer needs
only demand elasticity, not cost distributions. But aggregate demand elasticity in public procurement
is often the *worst*-measured object in the problem. Robustness has been moved, not eliminated.
The continuity of the guarantee in `δ` is reassurance, not a solution.

**Verifiability is the binding practical constraint.** `t(q) = z·u(q)` requires committing to pay a
fraction of a utility that is contractible in `q`. Where `q` is quality — the case the paper invokes
— this is precisely what procurement law and measurement cannot deliver, which is why scoring rules
exist. The mechanism is immediately usable for divisible quantity, much less so for quality.

**No moral hazard, and no renegotiation.** Laffont–Tirole and Rogerson put cost-reducing effort at
the centre, and sharing rules are exactly where effort incentives bite; here cost is exogenous
private information. The buyer is also assumed to commit fully — with ex post renegotiation the
constant share rule's rents for low-cost sellers (`z·u(q)` far exceeds cost) are an obvious target.

**The criterion is insensitive to competition.** `CRn = CR1` is a clean theorem, but a criterion that
cannot register the value of adding bidders is not measuring what procurement practitioners optimise.
Any coarse information — a cost bound, one moment, a reserve — breaks the knife-edge worst case and
the constant share rule stops being optimal.

## 7. How should researchers cite or refer to this paper?

Cite it as the reference for **constant utility share mechanisms attaining the competitive ratio in
prior-free procurement**, and as the robustness counterpart to Baron–Myerson (1982). It is the right
citation for three distinct claims: that a positive constant guarantee survives over all convex costs
(against the vanishing bounds of Hartline–Roughgarden); that the optimal robust rule is indexed by
demand elasticity alone; and that competition cannot improve a worst-case procurement guarantee.

Refer to it as Bergemann, Heumann and Morris, currently a revision (August 2026, Cowles Discussion
Paper 2479; an early version appeared as "Cost-Based Nonlinear Pricing", ACM-EC '23). Do **not** cite
it for optimal auction design or for anything involving effort, verification, or renegotiation — it
is silent on all three.
