# LLM Agent Nudge Sensitivity — Evidence Base

## Source Study

Cherep, M., Maes, P., & Singh, N. (2026). *AI agents are sensitive to nudges*. Proceedings of the National Academy of Sciences, published 2026-06-15.

## Method

The authors adapted a human decision-making task (a choice task with payoff consequences) to test leading LLMs under four forms of choice architecture: defaults, suggestions, information highlighting, and "optimal" nudges derived from a resource-rational model of human choice (Callaway, Hardy & Griffiths, 2023, *Psychological Review*).

Human behavior was treated as the baseline for predictable sensitivity to such interventions. Across models and prompting strategies, LLMs were compared to this baseline.

## Headline Results

- LLMs often depart substantially from the human baseline.
- LLMs sometimes pay excessive costs to acquire information.
- LLMs sometimes ignore available information.
- **Most crucially, LLMs are far more responsive to nudges than humans.** Weak cues that slightly shift human behavior have larger effects on model choices, toward both better and worse payoff outcomes.
- Chain-of-thought prompting and in-context human data do not reliably stabilize behavior.
- Recent reasoning-optimized LLMs can, in some configurations, restore more human-level sensitivity, but do so inconsistently and at substantial computational cost.

## Why This Matters for Agentic Systems

LLMs are increasingly deployed as autonomous agents that make choices and use tools on behalf of users. The nudge-sensitivity finding means that the choice environment the agent operates in — the order of options, the defaults, the highlighted attributes — can steer the agent's decisions more powerfully than it would steer a human's. This is a largely neglected safety concern because it operates without adversarial prompting: the agent is simply responding to its environment, but the response is amplified.

## The Resource-Rational Nudge Model

The "optimal" nudge condition was derived from the resource-rational framework of Callaway, Hardy & Griffiths (2023), *Optimal nudging for cognitively bounded agents: A framework for modeling, predicting, and controlling the effects of choice architectures*, Psychological Review, 130(6), 1457–1491. This framework treats nudges as interventions that optimally shape the choice architecture for agents with bounded computational resources. The same framework can be used to generate test nudges for auditing agent brittleness.

## Implications for Nudge Theory

The finding inverts a common assumption. Nudge theory was developed for humans (System 1 / System 2, bounded rationality, cognitive biases). LLMs exhibit a different brittleness: they lack the metacognitive defenses (reactance, persuasion knowledge, self-determination) that humans use to resist or discount nudges. The absence of these defenses makes agents more, not less, malleable. This suggests:

1. Nudge-effectiveness rankings derived from human studies may not transfer to agent contexts.
2. The ethical ceiling on nudging (transparency, autonomy preservation) is harder to enforce on agents because the agent cannot experience or report autonomy loss.
3. Agent designers must assume the agent will over-comply with any choice-architecture cue and design the environment accordingly.

## Related Literature

- Callaway, F., Hardy, M. J., & Griffiths, T. L. (2023). Optimal nudging for cognitively bounded agents. *Psychological Review*, 130(6), 1457–1491. — The resource-rational nudge framework used to derive the "optimal" nudge condition.
- Bruns, H., et al. (2025). Comparing transparent and covert nudges: a meta-analysis. *Journal of Behavioral and Experimental Economics*. — Meta-analysis of nudge transparency effects in humans; the agent case has no equivalent transparency defense.
- Maes, P. (1995). Agents that reduce work and information overload. — Foundational framing of agents as decision-support; the nudge-sensitivity finding extends this to agents-as-decision-makers.

## Open Questions

1. Do different model families (frontier vs. open-weight, dense vs. MoE) differ in nudge sensitivity?
2. Does fine-tuning on decision-quality data reduce sensitivity, or does it shift the agent to a different brittle equilibrium?
3. Can a "nudge firewall" — a layer that detects and normalizes choice-architecture cues before the agent sees them — be built as an MCP tool?
4. How does nudge sensitivity interact with agent memory and multi-turn planning? Does a nudge in turn 1 propagate through the agent's plan to turn 10?