**The one thing to learn from this paper:** in a dynamic screening problem the entire logic of static mechanism design survives, but the **inverse hazard rate is replaced by the inverse hazard rate multiplied by an impulse response**. The impulse response measures how much a later type still "remembers" the initial type; it is the single object that governs how large the distortion is in each period and how it evolves over time. Everything else in modern dynamic contracting — limited commitment, endogenous types, financial frictions, robustness — can be read as an answer to the question of what happens when this clean formula breaks down.

## 1. What the researchers do

This is a **survey**, not a new result: Alessandro Pavan reviews the theory of **dynamic mechanism design** (dynamic contracting more broadly), pulls out a few organizing lessons, and maps the open problems. It is written for someone who knows static mechanism design and wants a guided entry into the dynamic version.

The backbone is a deliberately stylized model in Section 2. A seller contracts with a buyer for T periods. In period t the buyer has a private marginal value **theta_t** for quantity q_t, values evolve according to Markov kernels F, both parties discount at delta, and the seller has convex cost C. The seller commits at t = 1 to a full dynamic mechanism; the buyer has "deep pockets", so he can be charged up front for rents he will only earn later.

From this Pavan builds three things in sequence:

- a **characterization of dynamic incentive compatibility** (Theorem in Section 2.2),
- a **dynamic virtual surplus** objective obtained by integrating the envelope condition, and
- a **distortion formula** showing how the wedge between contracted and efficient quantity moves over time.

Sections 4 to 9 then survey six active frontiers where the stylized model's assumptions are dropped one at a time: stochastic arrival and departure of agents (revenue management), limited commitment and renegotiation, joint screening and persuasion, endogenous type processes, ex-post participation and financial frictions, and non-Bayesian robustness.

## 2. Why the literature cares

Almost every long-lived contracting relationship an applied theorist cares about has the feature that **private information keeps arriving**. Multi-year supply and procurement contracts, income taxation over a career, insurance with evolving health and income, platform matching where users learn their own preferences, dynamic pricing of experience goods — in all of these the static Myersonian toolkit answers the wrong question, because it treats the agent's information as fixed at the contracting date.

The dynamic literature matters because it changes the *substantive* predictions, not just the algebra. Two examples from the survey: with **independent types and deep pockets, distortions vanish after period 1** entirely — the principal can sell future information rents up front, so there is nothing to gain by distorting later quantities. And under **limited commitment**, the Revelation Principle itself fails, because a principal who learns the agent's type wants to renege, and the agent anticipates this (the classic **ratchet effect**).

## 3. Why it is a contribution, and where it fits

The value here is **synthesis by an architect of the field**. Pavan (with Segal and Toikka) wrote the Econometrica paper that unified this literature in 2014; this survey is the readable version of that machinery plus a decade of follow-up, and it is explicit about what is *not* settled.

It sits alongside, and updates, the earlier surveys — Bergemann and Pavan's JET symposium introduction (2015), Pavan's World Congress piece (2017), and Bergemann and Valimaki (2019). What is new relative to those is the coverage of the post-2019 frontier: **Doval and Skreta's mediated-mechanism approach to limited commitment**, the "smart contracts" results of Brzustowski et al. (2023), **Makris and Pavan's recursive wedge formula** unifying new dynamic public finance with dynamic screening, and the robustness literature (Chassang; Libgober and Mu; Garrett et al. 2025).

The survey is honest about scope. It explicitly does **not** cover dynamic moral hazard, incomplete contracts, behavioral contract theory, or the empirical literature.

## 4. The key theorem, and the key picture

**The theorem (Section 2.2).** Define the **impulse response** I_(1),t(theta^t) as the expected derivative of the period-t type with respect to the period-1 type, conditional on the realized history. For an AR(1) process theta_t = gamma*theta_(t-1) + eps_t, this is simply gamma^(t-1). A mechanism is incentive compatible if and only if (a) an **envelope condition (ICFOC)** holds period by period — the derivative of the agent's continuation value with respect to his current type equals the impulse-response-discounted stream of future quantities — and (b) an **integral monotonicity condition (Int-M)** holds.

Part (b) is the substantive one. In static screening, implementability requires the allocation to be monotone in the report. Here it does **not**: quantity need not rise with each reported type. What must hold is that the *average* sensitivity of payoffs, weighted by impulse responses across states and time, is monotone. This weaker requirement is exactly what makes bandit-style mechanisms implementable, where a high report today triggers consumption that generates information *lowering* consumption tomorrow.

**The picture (equation 4).** With quadratic cost, the optimal quantity is

  q_t(theta^t) = max{ theta_t - [(1 - F_1(theta_1)) / f_1(theta_1)] * I_(1),t(theta^t) , 0 }.

Read it next to the static Myerson formula and the entire dynamic theory is visible in one line: **same inverse hazard rate, now multiplied by the impulse response**. Three immediate corollaries: no distortion at the top; downward distortions for every history below the top initial type; and **distortions shrink exactly as fast as impulse responses decay**. If types are independent across periods, impulse responses are zero after period 1 and the contract is first-best from period 2 onward.

## 5. Why this was hard and not done before

Three obstacles, all visible in the formula above.

First, **one-shot deviations are not enough**: an agent who lies today conditions all future reports on that lie, so the deviation space is a tree, not a line. Getting to a usable first-order condition required the impulse-response representation — writing theta_t as a deterministic function of theta_1 and a fixed sequence of shocks — which only works under regularity conditions on the kernels.

Second, **the relaxed program is a genuine gamble**. The Myersonian approach here drops all envelope conditions after period 1, all integral monotonicity conditions, and all but one participation constraint. Verifying afterwards that the dropped constraints hold is hard, and the survey is careful to note where the first-order approach is known to fail (Krahmer and Strausz's finite-type counterexample to Eso and Szentes; Battaglini and Lamba).

Third, **limited commitment breaks the standard canonical class**. Without commitment, direct incentive-compatible mechanisms are no longer without loss, and the field still does not know what the fully canonical class is. Pavan names this as open, along with the maximal surplus a non-committed seller can extract.

## 6. Where this would not do well

This is a survey, so the relevant caveats are about the framework rather than about a result.

- **Deep pockets do a lot of work.** The clean "no distortion after period 1 under independence" conclusion depends on the principal charging up front for future rents via a bond that can be very large. Add liquidity constraints and the conclusion goes away.
- **Commitment does even more work.** The bulk of the tractable theory assumes full commitment; the parts that do not are, by Pavan's own account, unresolved.
- **Single principal, mostly.** Dynamic contracting with **competing principals** under limited commitment is, as of this survey, essentially unstudied — a real gap for procurement and platform settings where suppliers face several buyers.
- **No transfers, no theory.** School choice, organ allocation and much public-sector rationing sit outside the quasilinear framework almost entirely.
- **Almost no empirics.** The survey's closing observation is that identification and estimation of models with *evolving* private information barely exists.

## 7. How to cite / refer to this paper

Cite it as the **current reference survey for dynamic mechanism design**, the way one cites Bergemann and Morris (2019) for information design. It is Pavan, A. (2025), "Dynamic Contracting", *Annual Review of Economics*, forthcoming; the working-paper version is dated August 2025.

Appropriate uses: as the citation for the impulse-response characterization of dynamic incentive compatibility when you do not want to re-derive it; as a pointer to the limited-commitment literature (Doval and Skreta 2022 and after); and as authority for the claim that a research question is open — Section 10 is an unusually explicit open-problems list. For the underlying theorem itself, cite **Pavan, Segal and Toikka (2014, Econometrica)** rather than the survey.
