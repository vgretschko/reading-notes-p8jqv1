**The one thing to learn from this paper:** if you let six different **model-free reinforcement-learning algorithms** bid against each other in multi-unit auctions, the **efficiency ranking of formats is robust but the revenue ranking is not**. Uniform-price is the most efficient format in every configuration tested, no matter how well the algorithms have learned. Revenue, by contrast, flips: with a mixed population of unequally competent learners, pay-as-bid raises the most when supply is ample; with a homogeneous population of well-trained agents, generalized second-price raises the most and pay-as-bid raises the least. The practical lesson is that **simulated revenue rankings are an artifact of how good your artificial bidders are**, and should not be quoted as format comparisons without that qualifier.

## 1. What the researchers do

Khezr and Taylor survey six **model-free reinforcement learning (RL)** algorithms and ask whether they are usable as artificial bidders for studying auction design. The six are: tabular **Q-learning**, **deep Q-learning (DQN)**, **vanilla policy gradient (VPG)**, **deep policy gradient (DPN)**, **advantage actor-critic (A2C)**, and **proximal policy optimization (PPO)**. For each they explain the mechanics and give a plain advantages/disadvantages table.

They then run an illustrative horse race. A seller offers K homogeneous units; six bidders each demand two units with private, diminishing marginal values drawn iid from U[0,10]. Three sealed-bid payment rules are compared:

- **Discriminatory price (DP)**, also known as pay-as-bid: winners pay their own bids.
- **Generalized second price (GSP)**: each winner pays the bid ranked immediately below their own.
- **Uniform price (UP)**: all winners pay the highest losing bid, i.e. the (K+1)-th highest bid.

Each of the six algorithms is one bidder. Each combination of format and supply (K = 4, 6, 8) is run for 100,000 episodes, after pre-training each algorithm separately against random bidders. Implementation is Gymnasium plus Stable Baselines 3, default hyperparameters, no tuning.

The diagnostic is what they call the **learning ratio**: (value minus bid) divided by value — essentially the proportional shading of a bid. A learning ratio that settles down means the agent has stopped exploring and converged on a strategy; one that keeps moving means it has not.

## 2. Why the literature cares

Multi-unit auctions are the case where **theory gives the least guidance**. Since Vickrey it has been known that single-unit intuitions do not carry over; with multi-unit demand there are multiple equilibria, demand reduction, and generally no closed-form bidding functions. The revenue and efficiency ranking of pay-as-bid versus uniform-price is famously unresolved — which is awkward, because that exact comparison is what treasury auctions and electricity markets have to decide.

The two existing empirical routes both have limits. Lab experiments are constrained by subject numbers and repetition. Structural empirics require an equilibrium selection to be assumed. **Simulation with learning agents** is the third route: it allows arbitrary numbers of bidders and units, and it does not require solving for an equilibrium. The existing economics work in this vein (Banchio and Skrzypacz 2022; Khezr, Mohan and Page 2024) uses simple Q-learning almost exclusively. This paper's stated purpose is to open the menu.

## 3. Why it is a contribution, and where it fits

The contribution is a **methodological one plus a cautionary benchmark**, not a new theorem.

Methodologically, it is a usable reference for an economist who wants to run RL bidders and does not know which algorithm to pick. The verdict: **PPO is the clear choice** — it converges fastest, holds the most stable learning ratio, and ranks first in payoff in almost every configuration. Tabular **Q-learning does surprisingly well** despite its simplicity, finishing top-three in several settings, which partly vindicates the existing literature's reliance on it. **DQN is the clear loser**, in one configuration earning a negative total payoff, which means it is systematically overbidding past value.

Substantively, the cautionary benchmark is the more useful output. Because the six algorithms differ in competence, the mixed-population runs are effectively an auction among bidders of heterogeneous sophistication — and the format ranking there differs from the ranking among uniformly skilled bidders. That gap is the paper's real finding, even though it is not the one advertised.

It fits into the growing algorithmic-bidding literature that Vitali's file already contains (Banchio and Skrzypacz on algorithmic collusion; Bichler and coauthors on learning agents in markets), but sits at the more applied, simulation-tooling end of it.

## 4. The key figure or result

The two revenue-and-efficiency tables, read together, are the paper.

**Mixed population (Table 5).** With K = 4 the three formats are close, UP slightly ahead on revenue. As supply rises the formats separate sharply: at K = 8, total revenue is **DP 1,400,719 > GSP 831,480 > UP 642,584** — pay-as-bid raises more than twice what uniform-price does. Mean efficiency is nonetheless highest under UP throughout (0.87, 0.88, 0.88 across K = 4, 6, 8), with DP flat at 0.83–0.84.

**Homogeneous population (Table 6, all six bidders PPO).** Efficiency rises for every format and nearly saturates (UP 0.98–0.99, GSP 0.97–0.99), and the **revenue ranking reverses**: GSP now leads at every supply level (for example 1,723,650 at K = 8 against DP's 1,267,547).

The mechanism is transparent once stated. The apparent revenue advantage of pay-as-bid in the mixed run comes from **weak learners overbidding** — DQN's negative payoff is the smoking gun — and pay-as-bid is precisely the format that converts an overbid directly into seller revenue. Fix the learning, and that revenue disappears. GSP emerges as the format whose performance is least sensitive to how competent the bidders are.

A second, smaller observation worth flagging: under **uniform price the algorithms learn to overbid on the first unit**, contradicting the standard theoretical result that bidding truthfully for the first unit is weakly dominant — and they earn higher payoffs doing so. The authors suggest this points to stable strategies outside the equilibria theory has catalogued.

## 5. Why this was hard and not done before

Multi-unit auctions have a **combinatorially large strategy space**: each bidder submits a vector of bids, so tabular methods run into the curse of dimensionality quickly, which is why the literature stayed with single-unit or knapsack settings. Getting six independent learners to converge in a **non-stationary environment** — each agent's environment is the other five agents, who are themselves still learning — is genuinely unstable, and the paper's honest reporting of which algorithms fail to settle is part of its value. There is also plain engineering: custom Gymnasium environments had to be built and Stable Baselines 3 modified for multi-agent use, and the whole exercise ran on GPU hardware over 54 training sessions.

## 6. Where this would not do well

The design choices are restrictive in ways that matter for anyone wanting to lean on the numbers.

- **Each episode is a single step.** The authors flag this themselves: PPO and A2C are built for problems where actions have multi-period consequences, so a one-shot auction per episode uses none of that machinery. Any conclusion about *dynamic* bidding, supply-function competition, or collusion sustained over repeated interaction is out of reach here.
- **No hyperparameter tuning, by design.** Defaults throughout. The algorithm ranking is therefore a ranking of out-of-the-box performance, not of potential. The authors concede higher compute might reorder it.
- **These are not equilibria.** The learning ratios converge; nothing establishes that the limit is a Bayes-Nash equilibrium or that it is unique. Reporting simulated revenue as a format comparison without that caveat would be a mistake — and the mixed-versus-homogeneous reversal shows exactly how much it costs.
- **Very small and very symmetric.** Six bidders, two units of demand each, values iid uniform on the same support. No asymmetries, no budget constraints, no reserve prices, no common-value component — so nothing here speaks to the winner's-curse dimension of treasury or spectrum auctions.
- **The reward function is a modelling choice.** Overbidding is penalized more heavily than losing, and rewards are normalized by value, which the authors note makes the induced utility concave. Risk attitude is therefore imposed, not learned.
- **No human validation.** Whether these strategies resemble anything a human bidder does is left for future lab work.

## 7. How to cite / refer to this paper

Cite it as a **methods survey**, not as evidence on auction format performance: Khezr, P. and K. Taylor (2026), "The Use of Artificial Intelligence for Auction Design", *Journal of Economic Surveys* 40(1), 269–285.

The natural uses are: as the citation for *which* RL algorithm to use when building simulated bidders (PPO, with tabular Q-learning as a cheap baseline); as support for the claim that RL simulation is a third route alongside theory and experiments in multi-unit settings; and — most usefully — as the citation for the warning that **simulated revenue rankings depend on the sophistication of the artificial bidders**, while efficiency rankings appear more robust. For the underlying multi-unit auction theory, cite Krishna (2009) or Ausubel et al. (2014) instead. Simulation data is posted by the authors on GitHub.
