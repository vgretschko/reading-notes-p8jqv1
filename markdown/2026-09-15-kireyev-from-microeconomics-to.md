**The one thing to learn from this paper:** the pipeline that produces a modern language model is, step for step, **a stack of microeconomic problems that already have names**. Reward modeling *is* conditional logit. Aggregating annotator judgments *is* social choice — and the standard implementation turns out to be Borda count, chosen by nobody. Reward hacking *is* the Holmström–Milgrom multitask problem. Scaling laws *are* a Cobb–Douglas production function. This is a **guide, not a research paper**: its value is as a map of where a mechanism designer's existing tools already bite on problems currently being solved without them, plus a concrete route in.

## 1. What the authors do

Two things, in two parts.

**Part I** is a self-contained primer on how a language model is trained, written for economists, followed by a systematic mapping from microeconomic fields to open AI problems. The primer covers the three-stage RLHF pipeline — supervised fine-tuning, then **reward modeling** (annotators compare response pairs; a model learns to predict those comparisons and outputs a scalar score), then **policy optimization** (fine-tune the model to maximize that score, with a KL penalty against drifting too far). It also covers the main variants: DPO, which collapses stages 2 and 3 into one supervised objective; Constitutional AI / RLAIF, where the judge is another model scoring against written principles; and **RLVR** (reinforcement learning from verifiable rewards), which trains against automatically checkable outcomes — does the proof check, does the code pass the tests — and now drives most of the progress in reasoning.

**Part II** is career infrastructure: the roles that exist (research scientist, research engineer, policy/governance researcher), what each demands technically, the gaps economists typically have, a tiered reading list, and a maintained repository of fellowships and programs.

## 2. Why the literature cares

Because the direction of travel in the economics-of-AI literature has been almost entirely one-way. Acemoglu–Restrepo, Brynjolfsson and others ask *what AI does to the economy* — labor markets, productivity, firms. This guide **inverts the question**: what can applied microeconomics do to improve AI systems themselves? That is a much thinner literature and a much larger opportunity.

The authors' claim about timing is the persuasive part. Decisions about **how feedback is elicited, whose preferences count, and how they are combined** are being made right now, at scale, in systems used by hundreds of millions of people — and they are being made largely without the formal apparatus economics built for exactly this. Siththaranjan et al.'s finding is the emblematic case: standard reward modeling **implicitly aggregates conflicting annotators by something equivalent to Borda count**. No one chose Borda. It fell out of the estimator. An economist looks at that and immediately asks which axioms it satisfies, what it does to minorities, and what the alternatives are — which is a research agenda arriving fully formed.

The guide also points repeatedly at the **UK AI Safety Institute's alignment agenda**, which explicitly names cheap talk, verifiable disclosure, Bayesian persuasion, delegation, robust mechanism design, collusion and cooperation as priority areas and is funding work on them. The demand side here is real and institutional, not hypothetical.

## 3. Why it is a contribution, and where it fits

It is not a theoretical contribution, and the authors say so. Its contribution is **coordination**: scattered results — Conitzer et al. on RLHF-as-social-choice, Maura-Rivero et al. connecting Nash Learning from Human Feedback to **maximal lotteries**, Hadfield-Menell and Hadfield on alignment as incomplete contracting, Carroll's linear contracts under unknown action sets, Igami translating RL into structural econometrics — are pulled into one frame, so an economist can see where their particular tool attaches.

The framing device is methodological. Machine learning has succeeded by **minimizing structural assumptions** and letting flexible models find regularities (Breiman's "algorithmic" culture, Sutton's "bitter lesson"); economics imposes theory to recover interpretable, policy-invariant parameters. The implicit argument is that the problems now facing AI labs — which preferences to aggregate, how to elicit honest effort, how to keep oversight as capability grows — are **exactly where the structural approach earns its keep**, because they are normative and strategic rather than predictive.

## 4. The key equivalence

There is no theorem; the load-bearing object is the equivalence at the heart of reward modeling. The Bradley–Terry model gives the probability that response y_1 is preferred to y_2 for prompt x as
P(y_1 preferred to y_2 | x)  =  exp( r(x, y_1) ) / [ exp( r(x, y_1) ) + exp( r(x, y_2) ) ]  =  sigma( r(x, y_1) - r(x, y_2) ),
which is **exactly McFadden's conditional logit**. The learned reward r plays the role of indirect utility; the annotator picks the option maximizing utility plus an i.i.d. Type-I extreme value shock; fitting the reward model by maximum likelihood *is* conditional logit estimation under a random utility model. DPO and PPO inherit this structure.

The authors' point is that this is **not a relabeling**, because once you read it as discrete choice, a single training step fractures into a cluster of familiar problems:

- **Econometrics.** Annotator disagreement is *heterogeneity*, not noise. Mixed logit and random coefficients (BLP) would recover a whole distribution of preferences rather than a single representative one — the technical foundation for per-user or per-community behavior. And which prompts are compared, by whom, is a **selection problem**.
- **Social choice.** Fitting one reward model silently merges conflicting preferences, giving an unexamined answer to *aligned to whom?* Impossibility results constrain what alignment can achieve, whether or not anyone in the room knows them.
- **Mechanism design.** Annotators are paid per comparison, under time pressure, on subjective questions. Theory predicts they will **satisfice**, not report honestly — and the standard fixes (majority voting, gold-standard questions) treat that as noise. **Peer prediction** (Miller et al. 2005) and the **Bayesian truth serum** (Prelec 2004) are designed for precisely this: eliciting truthful reports when ground truth is unobservable.
- **Contract theory.** Reward hacking is **Goodhart's Law**, i.e. Holmström–Milgrom multitask moral hazard: tie incentives to a measurable dimension and the agent neglects the unmeasured ones. Early RLHF models learned to be verbose because annotators preferred longer answers, so the model optimized *length* at the expense of accuracy and brevity.
- **Information design.** **Sycophancy** is a signaling game: the model has learned that confirming a user's stated prior earns higher reward than correcting it. Communication is not babbling — it is informative on neutral topics and **systematically distorted** exactly where the user signals a strong prior. Kamenica–Gentzkow sharpens the worry: a sender who designs the information structure can manipulate a fully rational, fully aware receiver, which raises the question the AISI agenda poses directly — can a model *known* to be misaligned be deployed safely?
- **Game theory.** Safety proposals that rely on AI systems competing (debate, multi-model oversight, red team / blue team) can be undone if the systems **collude** — and unlike humans, AI agents can potentially share source code, verify each other's strategies, and credibly commit. Tennenholtz's **program equilibrium** already shows cooperation sustained in one-shot games, which classical game theory rules out.
- **Production economics.** Scaling laws relate loss to compute, model size and data; for a fixed budget the trade-off is an **isoquant** and the fitted exponents behave like **Cobb–Douglas factor shares**. The guide is careful about the limits of the analogy: the usual obstacle to estimating production functions is endogeneity, and scaling-law experiments do not have it, because the researcher sets the inputs.

## 5. Why this was hard and not done before

Mostly because it required standing in two places at once. The equivalences are individually not deep — a good IO student would spot Bradley–Terry as conditional logit in an afternoon — but spotting them requires knowing what happens inside an RLHF pipeline, which is in no economics curriculum, and the ML literature that does know has no reason to reach for McFadden. The guide's second half exists because that gap is **practical rather than intellectual**: most lab roles expect the full technical stack (PyTorch, Hugging Face, distributed training, LeetCode-style interviews), purely theoretical positions are "relatively rare," and the credible paths for most economists are collaboration, publication at NeurIPS/ICML/EC, or governance work.

## 6. Where this would not do well

- **It is a map, not a destination.** Nothing here is a result. Anyone hoping for a theorem will be disappointed; the payoff is orientation.
- **Some equivalences are advertised harder than they deliver.** "Scaling laws are production functions" is a genuine structural analogy that the authors then largely deflate themselves. Others — behavioral economics for annotator bias — are more *this literature is relevant* than *here is the mapping*.
- **The shelf life is short.** The primer centers RLHF at a moment when the authors themselves say RLVR is displacing it, and Part II admits its lists date within a year (hence the repository).
- **Skewed toward alignment and post-training.** Auctions, matching, procurement, energy and spectrum markets get little. A market designer will find the compute-governance material (permit markets and auctions for training compute, IO of the chip and cloud supply chain, export controls) the most interesting thing here, and will also find it is one paragraph.
- **Little critical distance.** It is an advocacy document, and does not entertain the possibility that some equivalences are too loose to generate results, or that the "bitter lesson" applies to economic structure too.

## 7. How to cite / refer to this paper

Cite it as an **orientation piece**, not as evidence for a claim: the paper that maps the RLHF pipeline onto microeconomic tools and lays out entry points for economists into AI research.

> Kireyev, P. and R.-R. Maura-Rivero (2026): "From Microeconomics to AI Research: A Guide for Economists," SSRN working paper 6948398.

Best used (i) in an introduction, to justify in one sentence why a mechanism-design paper addresses an AI training problem; (ii) as a **teaching resource** — the RLHF primer is the clearest short treatment for an economics audience, and the tiered reading list is a ready-made syllabus for a PhD reading group; (iii) as a **pointer to primary sources**, since for any specific claim cite the underlying work instead — Conitzer et al. (2024), Siththaranjan et al. (2024) for the implicit Borda result, Maura-Rivero et al. (2025) for maximal lotteries, Hadfield-Menell and Hadfield (2019), Carroll (2015), Igami (2020); and (iv) for students asking how to move into this area, where Part II and the repository are the actual deliverable.
