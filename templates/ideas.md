# Experiment Ideas

**Metric:** [name] — [lower / higher] is better. **Keep threshold:** [X% improvement].
**Last updated:** run [N], [date]

All percentages in this file are improvement over the baseline at the time of the test. Positive = better, negative = worse, whichever direction the raw metric moves.

## Tier 1: High Confidence (try first)
[Ideas most likely to produce measurable improvement. Rank by expected impact.]

1. **[Experiment name]** — [Why this is likely to work. Reference learnings if available.]

## Tier 2: Speculative (try after Tier 1)
[Less certain but worth testing. Move to Tier 1 if early results are promising.]

1. **[Experiment name]** — [Hypothesis.]

## Tier 3: Diminishing Returns
[Ideas that might work but expected gains are small. Try only if Tier 1-2 are exhausted.]

1. **[Experiment name]** — [Why gains are likely marginal.]

## Near-Misses (improved, but below the keep threshold)
[Positive results too small to keep on their own. Test them in pairs — small gains can stack.]

- **[Experiment name]**: [+X% on baseline Y, run N]. [What it changed, why it might combine well.]

## Parked (failed, not enough evidence to bury)
[Failed once or twice with no clear root cause. Retry when the "revisit if" condition is met.]

- **[Approach name]**: [0/1 keeps, run N]. Revisit if: [what would have to change].

## Graveyard (do NOT retry unless the revisit condition is met)
[Entry requires 3+ failed attempts OR a known root cause. This section prevents wasted cycles.]

- **[Approach name]**: [0/3 keeps, runs N-M]. Why it fails: [root cause, not just "it regressed"]. Revisit if: [structural change that would remove the root cause, or "never"].
