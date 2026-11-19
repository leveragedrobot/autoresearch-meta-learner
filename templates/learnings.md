# [Project Name] Autoresearch — Learnings

**Metric:** [name] — [lower / higher] is better. **Keep threshold:** [X% improvement].
**Runs completed:** 0. **Last updated:** [date]

This file holds patterns, not a list of experiments. The queue, the near-misses and the graveyard live in ideas.md only. Keep this file under ~100 lines: when it grows past that, merge related entries instead of appending.

All percentages are improvement over baseline. Positive = better.

### Prior Knowledge
- [What you already know about the problem space]
- [Known constraints or limitations]
- [Previous manual tuning results, if any]

### What Works
[Include hit rate ("5/7 keeps") and the pattern behind it, not just the result.]

### What Doesn't Work
[The general lesson behind failures, so future runs don't retry slight variations. Specific dead approaches go in the ideas.md Graveyard.]

### Diminishing Returns Signals
[Track gains per lever across runs, e.g. "+30% → +19% → +8% → +2%". When gains shrink three times in a row, note it and name the next lever to try.]

### Structural Changes
[Big changes to the system under test, with run number. Each one is a trigger to re-check the "revisit if" conditions in ideas.md.]

### Open Questions
[Things that might change the approach if answered.]
