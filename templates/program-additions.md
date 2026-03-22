# Meta-Learner Additions to program.md

Add these sections to your existing autoresearch program.md.

## At the top (Setup phase)

Add after "Read program.md":

```
2. Read research/learnings.md and research/ideas.md FIRST
   - learnings.md contains cross-run knowledge: what works, what doesn't, graveyard
   - ideas.md contains prioritized experiment queue: start with Tier 1
   - Do NOT retry anything in the Graveyard section of ideas.md
```

## Inside the experiment loop

Add after the keep/discard decision:

```
  8. Update ideas.md: mark completed idea, re-rank if results suggest reprioritization
  9. Every 5 experiments: update learnings.md with patterns observed
     - Move confirmed failures to "What Doesn't Work" with hit rate (e.g., "0/3 keeps")
     - Note diminishing returns if improvement deltas are shrinking
     - Add near-misses (5-10% improvement) to combination candidates
```

## At run completion

Add a "Run Completion" section:

```
## Run Completion

When out of ideas or hitting diminishing returns:

1. Update learnings.md with full run synthesis:
   - Aggregate hit rates across all experiments
   - Identify the strategy shift points (where one approach stopped working)
   - Update diminishing returns signals

2. Update ideas.md:
   - Re-rank remaining ideas based on this run's results
   - Move exhausted approaches to Graveyard with reasons
   - Add new ideas inspired by results
   - Note near-misses for combination in the next run

3. Do NOT merge to main — send merge proposal for human review
```
