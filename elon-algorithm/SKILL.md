---
name: elon-algorithm
description: "Apply Musk's engineering algorithm to any skill, system, platform, codebase, config, or process: get it working, then ruthlessly question requirements, delete, simplify, optimize, accelerate, and automate — strictly in that order. Use when asked to 'run the elon algorithm', 'apply first principles', simplify/streamline/refactor a system, cut complexity or cost, reduce steps, or harden something that already works. The core discipline is ORDER: never optimize or automate a part that should have been deleted. Includes the delete-until-you-add-10%-back rule and a measured before/after report."
---

# The Elon algorithm

A repeatable order-of-operations for making anything better. The power is in the **order** — most people optimize and automate things that should not exist. Do the steps in sequence; iterate within a step until it's exhausted before moving on.

> Working baseline → ① question requirements → ② delete → ③ simplify & optimize → ④ accelerate → ⑤ automate

Automation is **last**, not first. Musk's own costliest mistake was automating before deleting.

## Step 0 — Make it work, and measure it
You cannot improve what you can't see. Before touching anything:
- Get a **functional baseline** — it must actually work end to end.
- **Measure** the things you intend to move: lines/files/parts, steps, latency, cost, tokens, failure rate, manual touches. Write the numbers down. Every later step is judged against this baseline.
- No baseline, no algorithm — you'd be guessing.

## Step 1 — Question every requirement
Requirements are the highest-leverage place to cut, because a deleted requirement deletes all the parts and work under it.
- Every requirement must trace to a **named person**, never a department or "the process." Go ask that person.
- Be **most** skeptical of requirements from smart/senior people — they get challenged least, so their dumb requirements survive longest.
- For each: *why does this exist? what breaks if it's gone? is it solving a real problem or an imagined one?*
- Kill the dumb ones. Fewer, sharper requirements before you build or fix anything.

## Step 2 — Delete (parts, code, steps, files, config)
Delete relentlessly, from **first principles** — not "what does this do?" but "**does this need to exist at all?**"
- Delete the part/step/file/flag/dependency/abstraction. Delete dead code and superseded paths outright.
- **The 10% rule:** if you don't later have to add ~10% of what you deleted back, you didn't delete enough. Some over-deletion is *required* — it's how you find the true minimum. Add back only what proves necessary.
- Delete the **requirement before the part**. Bias to removal; make others justify keeping.
- Exit criterion: iterate until removing one more thing breaks something real.

## Step 3 — Simplify & optimize what survives
Only now — on the parts that earned their place.
- Collapse duplication, merge overlapping paths, replace clever with obvious, flatten indirection.
- Optimize the hot path with data, not hunches. Optimizing a part that should've been deleted is the classic error — that's why this is step 3.
- Iterate until there's nothing left to simplify.

## Step 4 — Accelerate cycle time
Speed up what remains — throughput, latency, round-trips, feedback loops, iteration time.
- Batch, parallelize, cache, cut waits and hops. Shorten the build/test/observe loop.
- If a step is watched live, latency is UX — treat lag as a defect.

## Step 5 — Automate (last)
Only automate a process you have already questioned, deleted down, simplified, and sped up.
- Automating a bad process cements the bad process and makes it expensive to change. That's why it's last.
- Automate the toil that remains: setup, checks, repetitive edits, regressions (turn each fixed bug into a lint/test).
- Leave judgment steps manual.

## How to apply it (any target)
- **A skill / codebase:** baseline (LOC, files, deps, lint/tests) → question its scope → delete dead code, unused files, redundant flags/commands → simplify overlapping functions → speed up hot paths/round-trips → automate checks (lint/tests/CI).
- **A system / platform:** baseline (components, steps, latency, cost) → question each component's reason to exist → delete services/steps/config → consolidate → accelerate the critical path → automate provisioning/ops.
- **A process / workflow:** baseline (steps, handoffs, cycle time) → question each step's owner → delete steps and approvals → simplify handoffs → shorten cycle time → automate the mechanical steps.

## Corollaries (don't skip)
- **Whoever changes it must understand it hands-on** — no improving-by-proxy.
- **Move the constraint, not the comfortable part** — find the actual bottleneck and work there.
- **Beware false comradery** — being nice about a part's existence keeps dead weight alive. Challenge the work, not the person.
- **Reversible by design** — keep a way back (VCS, backup) so aggressive deletion is safe; that's what lets you over-delete and add 10% back.

## Output — the report
Deliver a phase-by-phase log with **before/after numbers**, so the win is provable:

```
BASELINE   parts/LOC · steps · latency · cost · failure-rate
① REQUIREMENTS  killed: <which, and whose they were>
② DELETED       parts/files/steps removed (with count); ~10% added back: <what>
③ SIMPLIFIED    merges/rewrites; complexity before→after
④ ACCELERATED   latency/round-trips before→after
⑤ AUTOMATED     toil removed; checks added (lint/test/CI)
RESULT     before → after on every baseline metric
```

If you did steps out of order (optimized or automated before deleting), say so and redo — the order is the method.
