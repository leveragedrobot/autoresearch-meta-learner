# autoresearch + meta-learner

> Based on [karpathy/autoresearch](https://github.com/karpathy/autoresearch). This is not a fork — it's an addition that layers persistent cross-session memory on top of the original pattern.

Three markdown files that give autonomous AI research loops cross-session knowledge.

## The Problem

Autoresearch works great for a single overnight run. But when you run multiple sessions — or apply the pattern across different projects — each session starts nearly blind. The agent might retry "SA parameter tuning" for the 4th time because it doesn't remember that it failed the first three.

## The Addition

Three files sit alongside the original `program.md` and `results.tsv`:

| File | Purpose |
|------|--------|
| `learnings.md` | What works, what doesn't, and why. Updated after each run. |
| `ideas.md` | Tiered experiment queue with a graveyard of dead ends. |
| `program.md` | Loop instructions (same core pattern as Karpathy's, with meta-learner update steps). |

No database. No embeddings. No RAG pipeline. The agent reads these at session start and updates them at session end.

## How ideas.md Works

```
## Tier 1: High Confidence (try first)
- Experiment A — reason it's likely to work

## Tier 2: Speculative (try after Tier 1)
- Experiment B — less certain but worth testing

## Tier 3: Diminishing Returns
- Experiment C — likely marginal gains

## Graveyard (do NOT retry)
- SA parameter tuning: 0/1 keeps across all runs, always regresses
- Quadratic penalty curves: inflates scores, regresses

## Near-Misses (5-10% improvement, save for combination)
- Experiment D: 7% improvement, below keep threshold
```

After each run, the agent re-ranks. Good ideas move up. Failed approaches get buried with a reason so no future agent wastes cycles on them.

## How learnings.md Works

```
### What Works
- Penalty reduction: 14/16 keeps. Most reliable lever.
  Pattern: reduce soft penalties aggressively, keep hard constraints.

### What Doesn't Work
- SA parameter tuning: 0/1 keeps. Not the bottleneck.

### Diminishing Returns
- Penalty reduction gains declining: 30% → 19% → 8% → 2%.
  Shift to structural changes.

### Near-Misses (combination candidates)
- Cross-day swaps: -3.7%. Might compound with other changes.
```

## What This Adds Over the Original

| Capability | Original | + Meta-Learner |
|-----------|----------|---------------|
| Cross-run knowledge | Context window only | `learnings.md` persists across sessions |
| Experiment prioritization | Agent freestyles | `ideas.md` tiered queue |
| Failed approach graveyard | "discard" in TSV | Explicit with reasons, never retried |
| Near-miss tracking | Discarded = forgotten | Saved for combination later |
| Diminishing returns | Grinds same lever | Detected, triggers strategy shift |
| Multi-session continuity | Each run starts fresh | Picks up where last run stopped |

## Results

Tested across 4 projects (scheduling solver, trading algo scanner, trading algo ranker, content filter API).

Scheduling solver over 3 autoresearch sessions:
- **Baseline:** 707,395 composite score
- **After 3 runs:** 14,600 composite (97.9% improvement)
- **Graveyard:** 8 dead ends no future run will repeat
- **Key insight from meta-learner:** penalty reduction hit a wall at run 3, but learnings.md identified "structural exclusion" as the next lever, reigniting gains (-29%, -44%)

## Usage

1. Set up [autoresearch](https://github.com/karpathy/autoresearch) (or any autonomous experiment loop)
2. Copy `templates/learnings.md` and `templates/ideas.md` to your `research/` directory
3. Add the sections from `templates/program-additions.md` to your `program.md`
4. Seed `learnings.md` with any prior knowledge about your problem space
5. Seed `ideas.md` with your initial experiment hypotheses, ranked by confidence
6. Run your autoresearch loop — the agent reads these files at the start and updates them after each run

## Templates

- [`templates/learnings.md`](templates/learnings.md) — empty learnings template
- [`templates/ideas.md`](templates/ideas.md) — empty ideas template with tier structure
- [`templates/program-additions.md`](templates/program-additions.md) — meta-learner sections to add to your program.md

## Credit

Core autoresearch pattern by [Andrej Karpathy](https://github.com/karpathy/autoresearch). Meta-learner addition inspired by his comments on the [No Priors podcast](https://www.youtube.com/c/NoPriorsPodcast) about needing to manually understand failure modes before automating improvement.
