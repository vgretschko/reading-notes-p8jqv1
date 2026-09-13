**The one thing to learn:** when a mechanism's own operation generates downstream data about the payoff-relevant state, that data can serve as the *ex-post statistic* that restores efficient implementation where the classical impossibility results say it cannot be done. In a conversational recommendation setting — an assistant that recommends, observes a purchase, exit or follow-up query, and recommends again — user feedback is an unbiased signal of her taste, and **paying advertisers contingent on that feedback rather than only on their reports** implements the efficient recommendation policy in periodic ex-post equilibrium, despite *multidimensional, interdependent* advertiser information. No external data is needed: the conversation supplies the statistic that disciplines it.

## 1. What the researchers do

A platform (an AI shopping assistant) interacts with one non-strategic user over T rounds, each round recommending one of n advertisers' products or none. Product i has commonly known characteristics gamma_i in R^k; the user has a private taste vector omega and gets utility gamma_i · omega from buying i (outside option zero). Advertiser i earns theta_it if her product is bought in t.

The information structure is the crux. Each advertiser has a **two-dimensional type**: her value theta_it, and a noisy signal s_it = omega + eps_it about the *user's* taste (third-party data, traffic on her own site). The platform holds a correlated signal of its own. Values are **interdependent**: which recommendation is efficient depends on what everyone knows about omega, not just on who values a sale most.

The user is mechanical: a known action rule maps (history, recommendation, omega) into probabilities of buying something already seen, exiting, or querying again. Each action generates a fresh platform signal about omega, so early rounds carry exploratory value and the efficient policy has a real explore/exploit structure.

The mechanism class is what the authors call **data-driven dynamic team mechanisms**. Allocations use only information available when the recommendation is made; *transfers may additionally condition on what happens afterwards*:

  p_it = −(gamma_i · omega-hat)·1{bought i} − sum over j≠i of (theta_jt + gamma_j · omega-hat)·1{bought j} + phi_it,

where phi_it is a free Groves term and omega-hat estimates the taste vector *after* the period-t outcome is realized — naturally, just the platform's own next-round feedback signal. Each advertiser thus becomes **residual claimant on realized flow social surplus**: a dynamic team mechanism (Athey-Segal 2013) with user utility replaced by a plug-in estimate from the conversation.

## 2. Why the literature cares

Two walls meet here. First, **Jehiel, Meyer-ter-Vehn, Moldovanu and Zame (2006)**: with multidimensional types and interdependent values, ex-post implementation of non-trivial allocation rules is generically impossible when both allocation and transfers depend only on messages. Second, ad markets are moving from keyword auctions with one-dimensional bids (GSP; Edelman-Ostrovsky-Schwarz, Varian) to conversational interfaces — Rufus, Google's AI shopping, ChatGPT Instant Checkout — where interaction is multi-round, advertisers know things about the user the platform does not, and the interaction itself produces a data exhaust.

## 3. Why it is a contribution, and where it fits

A clean cross of two lineages. From **Athey-Segal (2013)** and **Bergemann-Välimäki (2010)** it takes efficient dynamic mechanisms; from **Bergemann et al. (2025, EC)** it takes *data-driven* mechanism design, which showed statically that conditioning transfers on outcome data (realized click-through rates, say) restores implementation where message-driven mechanisms fail. The nearest antecedents for escaping the interdependent-values impossibility are **Mezzetti (2004)**, whose agents report realized payoffs in a second stage, and **Liu (2018)**, who identifies unbiased estimation of ex-post payoffs as sufficient dynamically.

The novel step: the estimator is neither an elicitation stage nor exogenous data, but is *endogenously produced by the mechanism's own interaction with a non-strategic third party*. Nobody reports their ex-post payoff — Mezzetti's weak point, since second-stage reports are only weakly incentivized — because the user's behaviour reports it for them.

## 4. Key theorem

**Theorem 1.** Under Assumptions 1-3, *every* data-driven dynamic team mechanism achieves periodic ex-post implementation of the efficient allocation rule.

The engine is Lemma 1. Because user utility is *linear* in omega and the estimator is (i) conditionally independent of the prior information history given (outcome history, omega) and (ii) unbiased, E[omega-hat | o_t, omega] = omega, one obtains E[(gamma_j · omega-hat)·1{bought j}] = E[(gamma_j · omega)·z_jt]: the plug-in estimate is, in expectation, exactly as good as knowing omega. Each agent's expected payoff then equals expected flow social surplus plus continuation surplus, up to a report-independent term, so truth-telling is optimal precisely because the allocation rule is efficient.

The complement is a **budget/participation tension** (Propositions 1-2): the Groves term phi+ = sup over own signals of W_1^{−i} gives no subsidy ex-post at the outset, phi− = inf gives ex-post individual rationality, and the two generally cannot be had together, since informational rents from interdependence can exceed an agent's allocative externality. Corollary 1 gives the knife-edge where they coincide: the platform's initial context is *sufficient* for omega relative to advertisers' signals.

## 5. Why this was hard

Three things had to line up. Interdependent values plus multidimensional types is exactly the Jehiel et al. regime, so any message-only construction is dead on arrival. Conditioning transfers on future data threatens the recursion, because a deviation today shifts the distribution of tomorrow's feedback: the estimator's properties must hold *conditional on every outcome history*, which is what Lemma 2's iterated-tower argument pins down. And Liu's (2018) fix required correlation between an agent's current type and others' future types — artificial. Recognizing that a conversational interface supplies a naturally occurring, history-indexed, unbiased signal of the common state, arriving *before* transfers settle, was unavailable before the interface existed.

## 6. Where this would not do well

This is a Yale-Google paper about advertising inside LLM assistants, and several assumptions should be read in that light.

**The welfare criterion is the platform's, not an economist's.** "Total surplus" is the sum of (theta_it + gamma_i · omega). There is no price in the model: the user pays nothing, advertiser margins enter surplus one-for-one with user match utility, and consumer surplus is never separated from advertiser rents. A high-margin, mediocre-match product can be "efficient," so any welfare or policy reading is unearned.

**The user is furniture.** She follows a known action rule, cannot be persuaded, evaluates gamma_i · omega perfectly on inspection, and never responds strategically to the recommendation policy. That assumes away what is contested about AI assistants: that they may *shape* preferences rather than reveal them. Endogenizing search would turn the problem into a fixed point and could overturn the efficient policy's explore/exploit structure.

**Unbiasedness does all the work and is empirically implausible.** The estimator is additive, in the same k dimensions as the taste vector, with mean-zero noise *conditional on every outcome history*. Real feedback is a click, an exit or a reformulated query — coarse, discrete, endogenously selected by the recommendation just made. That rules out the obvious selection problem: users who exit are a different draw from users who continue. If unbiasedness fails, truth-telling fails, and no bound in the size of the bias is offered.

**Nobody disciplines the platform.** It generates omega-hat, reveals it, and computes payments from it while absorbing the mechanism's deficit or surplus — and is modelled as non-strategic. For a platform with a revenue interest in the estimator, that is the least defensible assumption here.

**The clean case is the uninteresting case.** Corollary 1 reconciles participation and no-subsidy only when the platform's data is sufficient for omega — when advertisers' information is redundant and the elicitation problem the paper exists to solve does not arise.

**The conditioning event is manipulable.** Payments hinge on 1{purchase}, which advertisers can influence (self-purchase, returns, conversion fraud) as they cannot influence a reported signal. Team/VCG constructions are also collusion-prone, and no revenue comparison to GSP or auto-bidding is offered.

## 7. How to cite and refer to it

Cite as: Bergemann, D., M. Bojko, P. Dütting, R. Paes Leme, H. Xu and S. Zuo (2026), "From Conversations to Mechanisms: Aligning Advertiser Incentives in AI-Powered Product Recommendations," Cowles Foundation Discussion Paper No. 2513, Yale University. Not peer-reviewed; treat as a working paper.

Refer to it as the paper introducing **data-driven dynamic team mechanisms** — the dynamic extension of data-driven mechanism design — and as a constructive route around Jehiel et al. (2006) using *endogenously generated interaction data* rather than reported ex-post payoffs (Mezzetti 2004) or cross-agent type correlation (Liu 2018). Cite it for dynamic mechanism design with interdependent values, contingent transfers, and mechanism design for AI-mediated markets — not for empirical claims about LLM advertising or policy conclusions about recommendation platforms.
