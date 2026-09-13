**The one thing to learn from this paper:** "algorithmic collusion" — supra-competitive prices emerging from independent, self-learning pricing agents that were never programmed to collude — is best understood not as a new species of cartel but as a symptom of an old, unsolved problem in game theory: **we do not know which games learning dynamics actually learn**. The positive results are thin: **no-regret learning converges to the coarse correlated equilibrium (CCE) set, not to Nash equilibrium**, and CCE is large and can contain dominated strategies; Nash convergence is guaranteed only in restricted classes — **potential games**, **strictly monotone games** — that Bertrand competition generally does not belong to. Whether observed supra-competitive prices reflect genuine reward-and-punishment coordination or merely failure to converge hangs on that open question. This is an **overview and research-agenda article** (a BISE "Catchword" piece), not new theory or new evidence — read it as a map, and cite the primaries for results.

## 1. What do the researchers do?

Bichler, Durmann and Oberlechner write a short **synthesis** for the Business & Information Systems Engineering community, joining three literatures that rarely meet.

**Oligopoly pricing.** The workhorse is a normal-form **Bertrand** game with n firms, actions a_i (prices) and payoffs u_i(a_i, a_-i) = d_i(a)(a_i - c_i), under either **all-or-nothing** demand (lowest price takes all, ties split equally) or the **logit** demand of Calvano et al. (2020). They note the repeated-game extension and the **Folk Theorem**, and — importantly — that this literature benchmarks against the **stage-game Nash equilibrium**, not a repeated-game equilibrium.

**Online learning.** The single-agent problem: pick a price each period under **bandit feedback** (only the realised profit at the chosen price is observed), trading exploration against exploitation. The criterion is **regret** against the best fixed price in hindsight; **no-regret** algorithms such as **Exp3** drive average regret to zero. They separate **stochastic** from **adversarial** environments, and online learning from **reinforcement learning**, noting that in most of the collusion literature the state collapses to the current price, so "Q-learning" there is effectively an online learner.

**Learning in games.** The pivot. Single-agent guarantees do not transfer once every agent's action enters everyone else's payoff. Dynamics may **cycle, diverge or become chaotic**; no-regret dynamics land in CCE; Nash convergence is known for potential games (Cournot with linear price or cost qualifies) and strictly monotone games, but **Bertrand is generally neither**, and each demand specification is a different game with different convergence properties.

The survey then runs both ways. For collusion: Calvano et al. (2020), Klein (2021), Hansen et al. (2021, where **correlated exploration** by UCB agents generates supra-competitive prices), and Assad et al. (2024) — margins up **28%** in German retail gasoline duopolies after both firms adopted pricing software, with no effect in monopolies. Against: Abada et al. (2024b), where large epsilon-greedy exploration eliminates collusion; den Boer et al. (2022); and Eschenbaum et al. (2022), where offline-trained collusive policies break down when transplanted. The closing agenda has five headings: **algorithms**, **detection**, **regulation and monitoring**, **accountability**, and **beyond oligopoly** (ad auctions, two-sided platforms).

## 2. Why does the literature care?

Because it attacks a baseline assumption of applied game theory. In our models agents know enough to compute an equilibrium and play it **from period one**. In automated markets neither premise holds: firms do not know rivals' costs or the demand system, and computing Nash equilibrium is **PPAD-hard** in general (Daskalakis et al. 2009). Real agents therefore learn, and our equilibrium concepts do not tell us what markets of learners produce. The stakes are immediate: roughly a third of top Amazon products were algorithmically priced by 2015, and existing law, which requires explicit communication or agreement, may have nothing to bite on.

## 3. Why is it a contribution and where does it fit?

It is a **framing contribution**, not a results contribution. Three things it does that the primaries mostly do not:

- It **reframes algorithmic collusion as an instance of equilibrium learning**, in the lineage from Cournot (1838) and fictitious play (Brown 1951) through Hart and Mas-Colell (2006), rather than as an antitrust curiosity.
- It **connects the algorithm class to the game class**: the tendency toward supra-competitive prices is a joint property of the learner (feedback structure, exploration rate, synchronous vs. asynchronous updating — Asker et al. 2022) and of the game's structure. The decomposition is stated more cleanly here than in the sources.
- It **separates autonomous algorithmic collusion from hub-and-spoke arrangements**. The EU's 2023 horizontal cooperation guidelines make an agreement to use the *same* pricing algorithm an Article 101 infringement — a *different* problem, as the authors note.

It sits alongside den Boer (2023) and Abada et al. (2024a) as agenda-setting, with more weight on learning theory.

## 4. Key figure or organising claim

There is no theorem. The organising device is **Fig. 1, "Learning agents in different contexts"**, which nests three settings: one agent optimising against an exogenous stochastic or adversarial process (online learning); several agents optimising against each other in a fixed game (algorithmic collusion); and whether such dynamics reach equilibrium at all (equilibrium learning). The sharpest imported result is **no-regret-to-CCE** (Fudenberg and Levine 1999): guarantees that are strong for one agent become weak in a game, because the guaranteed set is large and may contain dominated strategies. That is what makes supra-competitive prices theoretically unsurprising and **ambiguous in interpretation**.

## 5. Why was this hard and not done before?

The underlying problem is genuinely open: there is no characterisation of "learnable" games, and the negative results (cycling, chaos, impossibility) suggest there will not be a simple one. The two required skill sets — regret analysis and online convex optimisation, oligopoly theory and antitrust — sit in different departments, so the literature fragmented into simulations that do not speak to convergence theory. The object is also hard to observe: field identification is thin, and simulations buy tractability by fixing the demand system, the number of firms and the algorithm — exactly what the theory says matters.

## 6. Where would this not do well?

- **It does not resolve the definitional problem it raises, though the policy discussion depends on that resolution.** Benchmarking against the *stage-game* Nash equilibrium lumps together two economically distinct phenomena: agents that have learned genuine reward-punishment strategies, and agents that have simply **not converged** — insufficient exploration, correlated exploration (Hansen et al.), or Lambin's "seeming collusion" from simultaneous experimentation. The paper cites both camps and declines to adjudicate. If most measured supra-competitive pricing is failure-to-converge, the antitrust framing is misplaced and the remedy is a technical standard, not a liability rule.
- **The defence of the Bertrand abstraction is asserted, not argued.** The claim that if collusion does not arise in a simple repeated Bertrand game it is "even less likely" in richer settings has no support and is plausibly backwards: demand fluctuation, entry and exit, and heterogeneous algorithms change the learning problem qualitatively, and non-stationarity blocks convergence as readily as it blocks collusion.
- **The evidence base is narrower than the framing suggests.** The paper concedes there is "little evidence that Q-learning is particularly important or widespread for algorithmic pricing" — yet Q-learning simulations carry most of the literature's weight. Policy may be calibrated on an algorithm nobody uses.
- **Thin on economics-side depth and on remedies.** No engagement with the identification debate around the gasoline evidence, and no treatment of **mechanism- or market-design responses** (tick sizes, price-commitment windows, disclosure rules, format choice). The auction side gets one self-citation. For a market designer, the agenda stops where design would begin.
- **The regulatory section ends in a gap rather than a proposal**: the 2024 US Senate bill is still a bill, DMA and DSA do not address the issue, and the EU guidance covers hub-and-spoke.
- **No treatment of LLM-based or general-purpose pricing agents**, where the practical frontier has moved and the online-learning taxonomy fits least well.

## 7. How should researchers cite or refer to this paper?

Cite it as a **survey and research-agenda piece**, not a source of results.

> Bichler, M., Durmann, J., and Oberlechner, M. (2025), "Algorithmic Pricing and Algorithmic Collusion," *Business & Information Systems Engineering* 67(6), 971–979. DOI: 10.1007/s12599-025-00965-z. (Open Access.)

Good uses: the framing that **algorithmic collusion is an instance of equilibrium learning**; the point that **learning guarantees degrade from Nash to CCE** once payoffs are interdependent; the **hub-and-spoke vs. autonomous collusion** distinction; and as an entry point for students. Do **not** cite it for empirical or simulation findings — cite Calvano et al. (2020, *AER*), Klein (2021, *RAND*), Hansen et al. (2021, *Marketing Science*), Assad et al. (2024, *JPE*) and Abada et al. (2024, *EJOR*) directly.
