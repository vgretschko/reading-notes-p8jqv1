**The one thing to learn:** pricing AI access looks hopeless — the user's private type is an entire *function* from a continuum of tasks to values, the provider sells *bundles* of tokens, and the user secretly reallocates them across tasks — yet one assumption, **homogeneity of the gain function**, collapses the problem to **one-dimensional Mussa-Rosen screening**. The type profile reduces to a scalar **aggregate type**, the hidden allocation adds **no incentive constraints**, and the optimum is a menu of **committed-spend contracts**: an upfront fee buys a budget the user spends freely on tokens priced at marginal cost. Tiered subscriptions, Poe's compute points, Copilot's premium requests with overages and linear pay-per-token APIs all fall out as indirect implementations of that one mechanism.

## 1. What do the researchers do?

A buyer faces a unit continuum of **tasks**; her private type is a function w: [0,1] -> [0,1], with w_i the marginal value of performance on task i. Performance is multiplicatively separable, g(x_i, z) = Psi(x_i) Phi(z), with x_i the J classes of **inference tokens** (input, output/reasoning) spent on task i and z the K classes of **fine-tuning tokens** improving the model on *all* tasks. Psi is strictly concave and **homogeneous of degree sigma in (0,1)** — a stand-in for inference-time scaling laws. The provider meters *totals*: it sells budgets (X, Z) at constant marginal cost per token class but cannot see the task-by-task split, combining **adverse selection over an infinite-dimensional type** with **moral hazard over the hidden allocation**.

Get the screening dimension right: it is *not* task composition, *not* the input/output mix. Homogeneity makes the cost-minimizing token mix **scale-invariant and task-independent** — every task uses inputs in identical proportions, only the scale w_i^(1/(1-sigma)) varies. Surplus and token demand then depend on w only through

  theta(w) = ( integral_0^1 w_i^(1/(1-sigma)) di )^(1-sigma),

an L_p norm of the type profile with p = 1/(1-sigma). Screening is **second-degree price discrimination in one index of aggregate compute quality**, Q = Psi(X) Phi(Z). Two users sharing theta — one intense about a few tasks, one moderate about many — are indistinguishable and treated alike.

Four steps follow: **capacity-constrained efficiency**, implementable by **linear prices equal to shadow-inflated marginal costs** (Cor. 1); the single-model monopoly menu, with exclusion and downward distortion below the top (Prop. 3); **multiple differentiated models**, where each type uses exactly **one model** and higher types get better ones — endogenous **versioning** (Props. 5-6); and a **leader facing an open-source fringe** (Prop. 7), where low types buy only from the fringe, intermediate types get just enough to deter top-up, and high types face full monopoly distortion.

## 2. Why does the literature care?

Multidimensional screening is the standing embarrassment of mechanism design: Armstrong (1996), Rochet-Stole (2003) and Daskalakis-Deckelbaum-Tzamos (2017) show optimal multiproduct nonlinear pricing is generically intractable, and moral hazard (Castro-Pires-Chade-Swinkels, 2024) makes it worse. Here both dissolve at once, in an economically motivated environment, for the decade's most consequential new market. Meanwhile the economics of AI had almost no theory of the provider's own **pricing and product-design problem**.

## 3. Why is it a contribution and where does it fit?

The contribution is a **sufficient-statistic reduction**, not a new technique. It belongs beside the "1.5-dimensional" mechanism design literature (Fiat et al., 2016; Devanur et al., 2020), with one step those papers do not need: ruling out incentive clashes across the hidden sub-allocations. The optimum is **constrained-efficient** as in Laffont-Tirole (1990) and Doligalski et al. (2025) — conditional on quality sold, the token mix is undistorted, so all distortion loads onto one scalar. Hence marginal-cost-priced budgets implement it, linking to Armstrong's (1996) **cost-based tariffs**.

## 4. Key theorem

**Proposition 2 (Buyer Indirect Utility)** is the hinge: for any budgets X, Z >= 0,

  U(w, X, Z) = theta(w) * Psi(X) * Phi(Z).

Everything else is a corollary. Private information enters through a scalar; the payoff is multiplicatively separable in type and allocation, which *is* the Mussa-Rosen (1978) form; and the unobserved within-bundle allocation adds **no incentive constraints**, since the cost-minimizing input mix is independent of scale and hence of type. With Lemma 1 (C strictly convex, C'(0+) = 0, so exclusion reflects information rents alone), the optimum solves phi-bar(theta) = C'(Q(theta)).

**Proposition 4 (Indirect Implementation)** completes the picture: the optimum is implementable as a **maximum-spend** mechanism (hard budget cap, tokens at marginal cost — Poe), and if the markup m(theta) = theta/phi-bar(theta) is decreasing (implied by an increasing hazard rate), also as a **minimum-spend** mechanism (a volume commitment unlocking lower per-token prices — Copilot's overages) and as a **menu of two-part tariffs**. Such a tariff must scale *all* marginal costs by the same m(theta): any other price vector distorts the input mix away from cost minimization.

## 5. Why was this hard and not done before?

Three difficulties stack: the type is a *function*, so demand-profile methods do not apply off the shelf; the allocation space is a continuum of J-vectors plus a K-vector; and reallocating a bundle is a hidden action, so each menu item spawns its own continuum of sub-constraints. Before inference-time scaling laws made metered token budgets the industrial object, there was no reason to write this production structure down. The technical step: the gradient ray of a homogeneous strictly concave Psi maps one-to-one onto a token ray, so the optimal mix is task-invariant — which buys both the aggregation and the vanishing of the moral hazard.

## 6. Where would this not do well?

**Homogeneity is not a regularity condition; it is the result.** Psi is common across tasks, so tasks differ *only* vertically by a scalar w_i. But the salient heterogeneity is horizontal: retrieval-heavy tasks are input-intensive, agentic coding output/reasoning-intensive. Let input shares vary by task and aggregation fails, as the authors concede. The paper assumes away precisely the multidimensionality that would make versioning and specialized models interesting, then reports versioning is easy.

**The moral hazard is vacuous, not solved.** Prop. 2 shows the hidden allocation imposes no constraints — clean, but the "combined adverse selection and moral hazard" framing oversells: under the maintained assumption no interaction is left to study. Regularity is also imposed on a *derived* object: the hazard-rate and markup conditions are assumed on the induced distribution of theta(w), not on primitives.

**The empirical mapping is illustration, not test — and one case cuts against it.** Prop. 3 predicts strictly falling average price per unit of quality, yet Poe's own table shows a *constant* $20 per million points for every tier above Basic. The paper reads Poe as confirming maximum-spend pricing while passing over that contradiction. More broadly, almost any menu of caps and tariffs fits *some* nonlinear pricing model.

**Cost and capacity are too clean.** Serving is lumpy, congestible and latency-sensitive: usage limits are per-5-hour *windows* and providers sell "priority capacity" — congestion instruments, not Mussa-Rosen quality distortions. Batch discounts (~50%) and prompt caching (~90%), declared outside the model, are first-order in real price lists and read as discrimination on *time and cache-hit rates*.

**No uncertainty.** Buyers know theta when choosing a plan, yet much of why real subscriptions pair non-rollover budgets with caps is **demand uncertainty and insurance**, not screening.

**The leader-fringe result is knife-edge and possibly backwards.** Closed forms require the fringe to have *zero* returns to fine-tuning, yet open-weight models are in practice the most fine-tunable option available — the opposite ranking is arguably relevant, and it reverses the assignment. Lemma 2 also forbids using two models on one task and Assumption 1 needs a common sigma, ruling out routers, cascades and ensembles. And the analysis is purely positive: exclusion is documented with no welfare accounting.

## 7. How should researchers cite or refer to this paper?

Cite it as **Bergemann, Bonatti and Smolin (2026), "Menu Pricing of Large Language Models," Cowles Foundation Discussion Paper No. 2502** (also CEPR DP21275, TSE WP 1670, arXiv:2502.07736). The EC'25 extended abstract under a different title is the *same* project; cite the current title only.

Refer to it as the paper showing **LLM pricing reduces to one-dimensional screening under homogeneous inference technology**. It is the canonical reference for: (a) the **aggregate-type sufficient statistic** in screening over general-purpose technologies; (b) the **equivalence of maximum-spend, minimum-spend and two-part-tariff implementations** of an optimal nonlinear tariff with marginal-cost-priced inputs; (c) **linear pay-per-token API pricing** as constrained-efficient under capacity limits; and (d) the **leader-versus-fringe** screening problem. Do not cite it as empirical evidence — Section 6 is an interpretive mapping, with Demirer et al. (2025) the empirical reference. The framework is explicitly portable to cloud computing.
