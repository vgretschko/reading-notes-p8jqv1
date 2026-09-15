**The one thing to learn from this paper:** when a principal talks to an agent through an AI intermediary that is wiped and restarted between periods, the agent is effectively an **imperfect-recall** decision maker who must use *one and the same* response rule at every stage — and even in that world a **revelation principle survives**. Anything the designer can achieve with an arbitrarily complicated dynamic signal structure, she can also achieve with a **direct mechanism that simply recommends actions**, in which obeying is (weakly) optimal. The catch, and the paper's real economic content, is that obedience must now be checked against *stationary* deviations rather than period-by-period ones. That turns implementability into a small, explicit system of inequalities — and makes **conditioning tomorrow's signal on today's realized action** a powerful design instrument in some environments and completely worthless in others.

## 1. What the researchers do

Picture a firm, platform or regulator (the **principal**) that never speaks to a customer directly. Every contact is handled by a chatbot, and for privacy, cost or product reasons each contact is handled by a **fresh copy** of the model, which sees nothing of what earlier copies said. From the human's point of view there is one counterpart sending her messages over time; from the modeling point of view, she ends up behaving as if *she* had imperfect recall, because she cannot tell one episode from another and so cannot condition on the history.

Formally: there are T periods. Each period the agent picks an action A_t from a finite set A (mostly A = {L, R}). Before acting she sees a signal S_t. The **designer's power is large and asymmetric**: the period-t signal may depend on the *entire* history of past signals *and* past realized actions. The **agent's power is small**: she must commit to a single stationary rule ell: S -> Delta(A), applied every period. The question is purely one of **implementability**: which joint distributions mu over action paths (A_1, ..., A_T) can arise from *some* signal structure together with a stationary best response?

The two-period binary case is then solved explicitly. The designer's instrument is five numbers: P_L (the probability of recommending L first) and four continuation probabilities alpha_L, beta_L, alpha_R, beta_R — the chance of recommending L in period 2 after each combination of first recommendation and realized first action. The alphas are the **on-path** continuations (the agent obeyed); the betas are the **off-path** ones (she disobeyed).

## 2. Why the literature cares

Two literatures meet here. The first is **information design / Bayesian persuasion** (Kamenica–Gentzkow; Bergemann–Morris; Doval–Ely on sequential information design), which almost always assumes a receiver with perfect recall — in which case the standard obedience machinery applies and the designer's problem is a linear program over distributions. The second is the old literature on **games with imperfect recall** (Piccione–Rubinstein, Aumann–Hart–Perry, the absent-minded driver), famous for breaking the usual equivalences: behavioral and mixed strategies diverge, dynamic consistency fails, and what the agent should believe becomes genuinely contested.

Nobody had asked what the *designer* can do when facing such an agent. That is now more than a curiosity, because **AI deployment makes imperfect recall a design choice rather than a cognitive defect**. Statelessness is the default for many deployed systems, and the AI-safety literature has begun using forced forgetting deliberately — running a model on a task without letting it know whether it is being tested or deployed. Once memory is a lever the principal pulls, the economics question is what pulling it buys you.

## 3. Why it is a contribution, and where it fits

The contribution is **methodological, and it is the right kind**: it restores a tool everyone already knows how to use. Revelation principles usually fail or need heavy qualification once agents' strategy sets are restricted, because "restrict to direct mechanisms" implicitly assumes the agent can consider all the deviations the designer worries about. Here the restriction is *stationarity*, and the authors show stationarity is preserved under the relevant composition: a stationary deviation in the direct mechanism can be **pulled back** into a stationary policy in the original one generating exactly the same joint law over action paths. The original best-response property therefore transfers, and obedience must be optimal.

Where it sits: a **mechanism-design counterpart** to AI-safety work on imperfect-recall testing (Chen–Ghersengorin–Petersen; Tewolde et al.), and a **constrained-receiver extension** of sequential information design. Short, clean, self-contained — a tool paper, not an applications paper.

## 4. The key theorem, and the key picture

**Theorem 1 (revelation principle under imperfect recall).** For any implementable outcome mu there is a direct mechanism whose signals *are* action recommendations R_t in A such that (i) under obedience the action path has distribution mu, and (ii) obedience is a weak best response — and can be made strict by punishing off path without changing mu.

The engine is **Lemma 2 (replication)**: if the agent deviates using rho: A -> Delta(A), define the composed rule ell'_a(s) = sum_r rho_(a|r) ell_r(s). Both mechanisms then generate the same joint law, term by term. That one line is the whole argument.

The most useful *picture* is **Proposition 2 and Figure 1**. In the absent-minded driver — two indistinguishable exits, payoff 1 for exiting at the second, a in [0, 1/2) for never exiting, 0 for exiting at the first — the payoff-relevant outcomes collapse to three, {R, LL, LR}, and the implementable set is a **convex polygon** in the simplex cut out by three linear constraints:

(1 - 2a) p_LL <= p_LR,    (1 + 2a) p_LL + 3 p_LR >= 1 + a,    a p_LL + p_LR >= 1 / (4(1 - a)).

At a = 0 this is the quadrilateral with vertices A = (2/3, 0, 1/3), B = (1/2, 1/4, 1/4), C = (0, 1/2, 1/2) and D = (0, 0, 1) in (p_R, p_LL, p_LR) coordinates. Since any linear principal objective is maximized at a vertex, the design problem reduces to reading off four points, each with a transparent implementation (A: recommend "exit now" with probability 2/3; C: always "continue", then a fair coin; D: guide her to her own favorite path).

The sharpest economic result is **Propositions 3 and 4** in the *mismatch* environment, where the agent is paid 1 whenever her two actions differ. With action conditioning, mu(L, L) <= 1/2 and the bound is **attained**. Without it (alpha_L = beta_L, alpha_R = beta_R) the same target is **impossible** — the KKT derivative at obedience is at most -1/2 < 0. Remark 3 gives the general principle: **action conditioning strictly expands the implementable set if and only if there is an outcome the agent reaches only after disobedience and over which she is not indifferent.**

## 5. Why this was hard and not done before

Imperfect recall has a bad reputation: the absent-minded-driver literature is mostly about paradoxes of belief and dynamic inconsistency, so the instinct is that nothing as clean as a revelation principle can survive. The restriction also bites in an unfamiliar direction — the usual worry is that the agent knows *too much*, whereas here the binding constraint is that **a single rule must do double duty**, so the parameters shaping on-path outcomes and those deterring deviations are entangled. Finally, letting the period-2 signal condition on the **realized action** is not standard in persuasion (signals normally depend on the state and past *messages*), and it is exactly that feature which generates the paper's asymmetry between environments.

## 6. Where this would not do well

- **It is an implementability characterization, not an optimal-design theory.** It tells you which mu are reachable, not how to solve the principal's problem for a general objective.
- **The geometry is worked out only for T = 2 with two actions.** The revelation principle holds for general finite T and A, but the constraint system grows fast and nothing guarantees a tractable polytope.
- **The agent has known payoffs and no private information.** No type space, no participation constraint, no transfers — anyone hoping to plug this into a screening or procurement problem will find the scaffolding absent.
- **The behavioral premise is strong.** "Fresh copy of the model, therefore the human uses a stationary rule" is a leap: real users learn across episodes even when the system does not. The constraint is imposed, not derived.
- **Some proofs are sketched** (sufficiency in Proposition 2 especially), and the draft carries visible rough edges.

## 7. How to cite / refer to this paper

Cite it as the paper establishing a **revelation principle for information design when the receiver has imperfect recall and must play a stationary rule**, and identifying **action-contingent continuation of signals** as an instrument whose value turns on whether the agent cares about outcomes reachable only after disobedience.

> Levy, M. and B. Szentes (2025): "Information Design for AI Proxies under Imperfect Recall," working paper.

Natural uses: as a **licence to work with obedient recommendation mechanisms** wherever the receiver's strategy is forced to be stationary (memoryless AI intermediaries, stateless algorithmic advice, repeated interactions with anonymized users); as the source of the **if-and-only-if condition on when conditioning signals on past actions pays**; and as a bridge citation connecting economic information design to AI-safety work using forced forgetting as a control device. For the canonical antecedents instead, cite Doval–Ely (2020) and Piccione–Rubinstein (1997).
