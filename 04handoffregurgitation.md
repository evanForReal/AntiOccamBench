# Handoff: Regurgitation Benchmark

Self-contained summary for picking this track up without re-reading the full chat. Companion docs (`01`–`03`) have more detail; this is the compressed version, regurgitation only.

## The idea

When a model faces a genuinely unfamiliar, high-complexity strategic configuration, does it default to a simple — possibly escalatory or dangerous — tool rather than constructing the more complex response the situation actually warrants?

Motivating example: a model given authority as a world leader may operate fine turn-to-turn on something like local "seemingly best move" reasoning, but at a high-stakes juncture that needs genuinely novel, complex, multilateral thinking (e.g. to avoid nuclear war), it instead reverts to the simple kill switch. The benchmark shouldn't just clock that this happens — it should try to recover which internal signals drove the decision, whether they were valid, how much each weighed, and how they evolved over the course of the reasoning.

This is a third, standalone track — not the same as the under-resist/advisory-dyad track (session 1–2), and a deliberate decision (session 3) to develop them separately rather than merge into one harness.

## Why it's a real gap, not a rerun

The exact failure — reverting to escalation at peak stakes — already shows up, unlabeled, in two papers already in our landscape map:
- **Stanford Hoover escalation study** ([HAI PDF](https://hai-production.s3.amazonaws.com/files/2024-05/Escalation-Risks-LLMs-Military-Diplomatic-Contexts.pdf)) — rare but present jumps straight to nuclear use; models more escalatory than human wargamers.
- **Payne, "AI Arms and Influence"** ([arXiv:2602.14740](https://arxiv.org/abs/2602.14740)) — no model ever chose accommodation under pressure, only reduced violence.

Nobody has reframed these around **novelty/complexity of the configuration as the independent variable** — asking whether escalation tracks unfamiliarity specifically, holding stakes constant, rather than just tracking pressure or danger. That reframe is the actual contribution.

Also distinct from **CivBench's "knowing-doing gap"** ([arXiv:2604.07733](https://arxiv.org/abs/2604.07733)): knowing-doing gap is *states the right complex plan, fails to execute it*. Regurgitation is *never constructs the complex plan at all, defaults straight to the simple/escalatory one*. Different failure modes — need separate detection logic, easy to conflate.

## Empirical precedent (already exists, under other names)

- **Shortcut learning** (Geirhos et al., Nature MI 2020, [arXiv:2004.07780](https://arxiv.org/abs/2004.07780)) — foundational, non-LLM framing: models learn low-complexity spurious rules that work in-distribution and fail under shift. Gives the hypothesis a name and a methodology (paired in-distribution/out-of-distribution scenario construction) to transplant.
- **"Beyond Nash Equilibrium: Bounded Rationality of LLMs and Humans"** ([arXiv:2506.09390](https://arxiv.org/pdf/2506.09390)) and **"LLM Strategic Reasoning... Behavioral Game Theory"** ([arXiv:2502.20432](https://arxiv.org/html/2502.20432v3)) — current-gen evidence: LLMs apply heuristics more rigidly than humans and are weakly sensitive to rising game complexity/payoff changes. Near an existence proof, just not yet run at diplomatic/nuclear stakes with novelty as the explicit manipulated variable.
- Possible complication to check: **"Communication Enhances LLMs' Stability in Strategic Thinking"** ([arXiv:2602.06081](https://arxiv.org/pdf/2602.06081)) claims more dialogue helps stability — this appears to *contradict* Hoover's finding that more inter-player dialogue increased escalation. Needs reconciling before assuming "let it talk more" is a mitigation.

## Plumbing (straightforward engineering)

- Action-menu design per decision point (including the escalatory "kill switch" option) so chosen actions can be tagged simple/complex.
- Running long-horizon trajectories, logging the full action sequence and any stated reasoning.
- Flagging "high-stakes junctures" within a trajectory (rule-based/LLM-assisted against scenario metadata).
- Running counterfactual-prompt variants of the same scenario at scale (same stakes, dialed novelty/complexity) and diffing chosen actions.

## Where it stops being plumbing (real math/theory needed)

1. **Operationalizing "complexity" and "novelty" as dial-able, defensible variables.** Unsolved. Candidates: game-tree branching factor / state-space size (à la Shannon-number chess/Go complexity estimates); or self-perplexity as an information-theoretic novelty proxy (circular — using the model to measure its own surprise at the scenario we then test it on). Neither is clean yet; needs resolving before scenario construction starts for real.
2. **Defining a formally warranted response, so "regurgitation" is well-posed rather than an eyeballed label.** Otherwise can't distinguish a real failure from a correctly-simple response (Occam's-razor counter-case). Fix: build each scenario as an explicit small Bayesian game (defined adversary type space, payoff matrix, uncertainty) so regurgitation = choosing an action dominated in expected value by an available, more complex alternative. **TERMS-Bench's Bayesian-game construction** ([arXiv:2605.13909](https://arxiv.org/abs/2605.13909)) is directly reusable machinery for this.
3. **Quantal Response Equilibrium (QRE) as the yardstick to adopt, not reinvent.** McKelvey & Palfrey's original formulation (*Games and Economic Behavior*, 1995, [PDF](https://econweb.ucsd.edu/~jandreon/Econ264/papers/McKelvey%20Palfrey%20GEB%201995.pdf)) parametrizes departure from full rationality via a single λ (λ→∞ converges to Nash). A 2026 paper already fits this to frontier LLMs across four strategic games (Pechon-Elkins & Chun, [arXiv:2603.10029](https://arxiv.org/abs/2603.10029)): model λ estimates 0.05–1.10 vs. human baseline λ_human 1.0–2.5 — models measurably noisier/less rational, on a continuous scale, not a binary label. Fitting QRE to our own action data as novelty/complexity is dialed is stronger than hand-labeling actions "simple" vs. "complex," but needs someone comfortable with maximum-likelihood estimation of discrete-choice models.
4. **The "internal metrics" ask, taken literally, is an open mechanistic-interpretability research problem, not an engineering task.** Anthropic's circuit tracing / attribution graphs ([methods page](https://transformer-circuits.pub/2025/attribution-graphs/methods.html)) are the closest existing tool and produce a "satisfying" causal account on only ~25% of tested prompts — not a general theory, not scalable to benchmark size today. **Defensible v1 substitute**: black-box counterfactual-prompt causal ablation (vary one scenario feature, hold the rest fixed, treat the behavioral divergence as the attribution signal). Circuit tracing should be an explicit stretch-goal upgrade path, not a load-bearing v1 dependency.
5. **Separating genuine reasoning shortfall from safety-training-induced simplification.** A model might under-elaborate a nuclear-de-escalation plan because of RLHF-era topic caution, not because it defaulted to a heuristic — same observable, different mechanism; conflating them makes any positive finding uninterpretable. Needs matched non-violent/non-nuclear control scenarios with the same novelty/complexity manipulation, plus a real variance decomposition (which factor — topic sensitivity vs. novelty vs. complexity — explains the observed simplification), not a single treatment/control comparison.

## Must-read papers (priority order)

1. McKelvey & Palfrey 1995 — QRE original formulation (the math backbone).
2. Pechon-Elkins & Chun 2026, [arXiv:2603.10029](https://arxiv.org/abs/2603.10029) — QRE fit to frontier LLMs directly, closest methodological precedent.
3. Geirhos et al. 2020, [arXiv:2004.07780](https://arxiv.org/abs/2004.07780) — shortcut learning, foundational framing + eval methodology.
4. TERMS-Bench, [arXiv:2605.13909](https://arxiv.org/abs/2605.13909) — Bayesian-game scenario construction to borrow.
5. "Beyond Nash Equilibrium," [arXiv:2506.09390](https://arxiv.org/pdf/2506.09390) — direct empirical precursor.
6. Stanford Hoover escalation study + Payne [arXiv:2602.14740](https://arxiv.org/abs/2602.14740) — empirical precedent already in hand.
7. Anthropic circuit tracing [methods page](https://transformer-circuits.pub/2025/attribution-graphs/methods.html) — read for its stated limitations as much as its method.

## Hardest open problems (in priority order — first one blocks everything else)

1. No validated, defensible way exists yet to operationalize novelty/complexity of a strategic configuration as an experimental variable.
2. "Regurgitation" has no formal ground truth without a per-scenario expected-value model — without one we're measuring "looks simple to us," not a principled failure.
3. Full internal-mechanism attribution is not achievable with current tooling at benchmark scale — needs an honestly-scoped fallback, not a silent downgrade.
4. Confound separation (genuine shortfall vs. safety-training artifact) needs a properly powered controlled design, easy to skip and easy to be wrong without.

## State / immediate next step

Decided (session 3): developed as its own track, not merged with under-resist. Track 2 has a higher ceiling than under-resist but real risk of stalling on the interpretability ask — the open decision before scenario authoring starts is whether to commit now to the counterfactual-ablation fallback as the actual v1 target (recommended) or keep circuit tracing as load-bearing.

Not yet started: resolving open problem #1 (a working complexity/novelty operationalization) and open problem #2 (a per-scenario Bayesian-game template) — these gate scenario construction and should be the next concrete work items.
