# autoresearch + meta-learner

> Based on [karpathy/autoresearch](https://github.com/karpathy/autoresearch). This is not a fork — it's an addition that layers persistent cross-session memory on top of the original pattern.

Two markdown files, plus a few lines added to your `program.md`, that give autonomous AI research loops cross-session knowledge.

## The Problem

Autoresearch works great for a single overnight run. But when you run multiple sessions — or apply the pattern across different projects — each session starts nearly blind. The agent might retry "SA parameter tuning" for the 4th time because it doesn't remember that it failed the first three.

## The Addition

Two new files sit alongside the original `program.md` and `results.tsv`, and `program.md` gets a few extra steps:

| File | Status | Purpose |
|------|--------|---------|
| `learnings.md` | New | Patterns: what works, what doesn't, where gains are fading. |
| `ideas.md` | New | Tiered experiment queue, near-misses, parked ideas, and a graveyard of dead ends. |
| `program.md` | Yours, extended | Same loop as Karpathy's, plus steps that read and update the two files. |

No database. No embeddings. No RAG pipeline. The agent reads these at session start and updates them as it goes.

Each fact lives in one place. `ideas.md` tracks specific experiments. `learnings.md` tracks the patterns behind them.

## One Convention for Numbers

Every percentage is **improvement over baseline: positive is better, negative is worse**. This holds whether your raw metric goes down (a penalty score) or up (accuracy). Each file states the metric, its direction, and the keep threshold at the top so the agent never has to guess.

## How ideas.md Works

```
## Tier 1: High Confidence (try first)
- Experiment A — reason it's likely to work

## Tier 2: Speculative (try after Tier 1)
- Experiment B — less certain but worth testing

## Tier 3: Diminishing Returns
- Experiment C — likely marginal gains

## Near-Misses (improved, but below the keep threshold)
- Experiment D: +3.7%, below the 5% keep threshold. Might stack with E.

## Parked (failed, not enough evidence to bury)
- Cross-day swaps: 0/1 keeps. Revisit if: the solver gets a new move type.

## Graveyard (do NOT retry unless the revisit condition is met)
- SA parameter tuning: 0/3 keeps. Why: not the bottleneck, score is
  dominated by penalties. Revisit if: penalties stop dominating the score.
- Quadratic penalty curves: 0/2 keeps. Why: inflates scores by
  construction. Revisit if: never.
```

After each experiment, the agent re-ranks. Good ideas move up. Failures move down, with rules that keep the graveyard honest:

- **One failure is not a burial.** An idea that failed once or twice with no clear cause is *parked*, not buried.
- **Burial needs evidence.** The graveyard takes 3+ failed attempts or a known root cause.
- **Every entry says what would bring it back.** An idea that failed early can work after the system changes. When a run makes a structural change, the agent re-checks each "revisit if" condition and moves matching ideas back into the queue.

## How learnings.md Works

```
### What Works
- Penalty reduction: 14/16 keeps. Most reliable lever.
  Pattern: reduce soft penalties aggressively, keep hard constraints.

### What Doesn't Work
- Tuning the search itself: 0/3 keeps. The search isn't the bottleneck.

### Diminishing Returns Signals
- Penalty reduction gains fading: +30% → +19% → +8% → +2%.
  Shift to structural changes.

### Structural Changes
- Run 3: added structural exclusion. Re-check revisit conditions.
```

The file is capped at about 100 lines. The agent merges related entries instead of appending forever, so reading it stays cheap in every future session.

## What This Adds Over the Original

| Capability | Original | + Meta-Learner |
|-----------|----------|---------------|
| Cross-run knowledge | Context window only | `learnings.md` persists across sessions |
| Experiment prioritization | Agent freestyles | `ideas.md` tiered queue |
| Failed approaches | "discard" in TSV | Buried with a reason and a revisit condition |
| Near-miss tracking | Discarded = forgotten | Saved and tested in combination |
| Diminishing returns | Grinds same lever | Detected, triggers strategy shift |
| Multi-session continuity | Each run starts fresh | Picks up where last run stopped |
| Crash safety | Results in TSV only | `ideas.md` updated after every experiment |

## Results

**Read these as a case study, not proof.** There was no side-by-side run of plain autoresearch without the extra files, so the gain below cannot be credited to the meta-learner alone. The raw run logs are not in this repo.

Scheduling solver over 3 autoresearch sessions (composite penalty score, lower is better):

- **Baseline:** 707,395
- **After 3 runs:** 14,600 (97.9% improvement)
- **Graveyard:** 8 dead ends
- **Where the files visibly helped:** penalty reduction hit a wall in run 3. `learnings.md` had flagged the fading gains and named "structural exclusion" as the next lever. The two experiments that followed improved the score by 29% and 44%.

The pattern has also been used on three other projects (a trading scanner, a trading ranker, a content filter API). No numbers are published for those.

A fair test would run the same project twice from the same baseline, with and without the files, for the same number of sessions. Results from anyone who tries this are welcome.

## Usage

1. Set up [autoresearch](https://github.com/karpathy/autoresearch) (or any autonomous experiment loop)
2. Copy `templates/learnings.md` and `templates/ideas.md` to your `research/` directory
3. Add the sections from `templates/program-additions.md` to your `program.md`
4. Fill in the metric, its direction, and the keep threshold at the top of both files
5. Seed `learnings.md` with any prior knowledge about your problem space
6. Seed `ideas.md` with your initial experiment hypotheses, ranked by confidence
7. Run your autoresearch loop — the agent reads these files at the start and updates them as it goes

## Templates

- [`templates/learnings.md`](templates/learnings.md) — empty learnings template
- [`templates/ideas.md`](templates/ideas.md) — empty ideas template with tiers, parked list and graveyard
- [`templates/program-additions.md`](templates/program-additions.md) — meta-learner sections to add to your program.md

## Credit

Core autoresearch pattern by [Andrej Karpathy](https://github.com/karpathy/autoresearch). Meta-learner addition inspired by his comments on the [No Priors podcast](https://www.youtube.com/c/NoPriorsPodcast) about needing to manually understand failure modes before automating improvement.

## License

[MIT](LICENSE)
