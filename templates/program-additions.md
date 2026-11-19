# Meta-Learner Additions to program.md

Add these sections to your existing autoresearch `program.md`. Step numbers are left out on purpose: slot each block into your own numbering.

## Setup phase

Add right after the step where the agent reads `program.md`:

```
- Read research/learnings.md and research/ideas.md BEFORE planning any experiment
  - learnings.md: patterns from past runs (what works, what doesn't, where gains are fading)
  - ideas.md: the experiment queue. Start with Tier 1.
  - Do NOT retry anything in the Graveyard unless its "revisit if" condition is now true
  - If learnings.md lists a structural change since an idea was parked or buried,
    check that idea's "revisit if" condition. If met, move it back to Tier 2.
```

## Inside the experiment loop

Add right after the keep/discard decision:

```
- Update ideas.md now, before the next experiment (a crashed run must not lose results):
  - Kept: remove from the queue, re-rank related ideas
  - Improved but below the keep threshold: move to Near-Misses with the number
  - Failed once or twice, cause unknown: move to Parked with a "revisit if" condition
  - Failed 3+ times, or the root cause is known: move to Graveyard with the reason
    and a "revisit if" condition (or "never")
- State every result as improvement over baseline. Positive = better.
- Every 5 experiments: update learnings.md with patterns, not individual results
  - Hit rates per lever (e.g. "penalty reduction: 4/5 keeps")
  - Diminishing returns if a lever's gains shrank three times in a row
```

## At run completion

Add a "Run Completion" section:

```
## Run Completion

When out of ideas or hitting diminishing returns:

1. Update learnings.md with the run synthesis:
   - Hit rates per lever across all runs
   - Strategy shift points (where one lever stopped working and what replaced it)
   - Any structural change made this run, under "Structural Changes"
   - Merge related entries so the file stays under ~100 lines

2. Update ideas.md:
   - Re-rank remaining ideas based on this run's results
   - Add new ideas inspired by results
   - Queue the most promising pair of Near-Misses as a Tier 1 combination test
   - Re-check every Parked and Graveyard "revisit if" condition against this run's changes

3. Do NOT merge to main — send a merge proposal for human review
```
