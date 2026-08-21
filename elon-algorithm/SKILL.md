---
name: elon-algorithm
description: "Apply Musk's engineering algorithm to any skill, system, codebase, config or process: get it working, then question requirements, delete, simplify, optimize and automate - strictly in that order, each step repeated until a fresh pass finds nothing. Use when asked to run the elon algorithm, apply first principles, simplify or streamline a system, cut complexity or cost, reduce steps, root out inefficiency, or harden something that already works. Never optimize a part that should have been deleted. Starts from the CURRENT documentation of the platform in use, not a training-data snapshot of it. Runs two clocks - per action and end-to-end - because only the second decides. Treats a fix that depends on someone remembering as a wish, and demands evidence the behaviour actually moved."
---

# The Elon algorithm

A repeatable order-of-operations for making anything better. The power is in the **order** — most people optimize and automate things that should not exist.

> Working baseline → ① question requirements → ② delete → ③ simplify → ④ optimize → ⑤ automate

Automation is **last**, not first. Musk's own costliest mistake was automating before deleting.

## The two rules that make it work

**ORDER.** Never optimize or automate a part that should have been deleted. Deleting a requirement deletes every part and every optimization underneath it, so requirements come first and automation comes last.

**EXHAUSTION.** A step is finished when a **fresh pass over it finds nothing** — not when you run out of ideas or the obvious wins are gone. A pass that found something is proof the step is not done: run it again. Each step below therefore ends with an explicit exit test, and you do not advance until it holds.

Stopping after one pass per step is the single most common way this fails. Two complete runs of this algorithm over one system still left an 11.2s call that existed only to discover a process the system had started itself, and ~3500 tokens per run of an agent transcribing facts it had already recorded — because "accelerate" had been *done once*.

## The fix must not depend on remembering

**If a fix requires a person or an agent to behave differently next time, it is not a fix — it is a
wish.** Write it down by all means, then assume it will not happen and change the system so the
behaviour is unnecessary.

This is not pessimism, it is arithmetic. Measured: an instruction to batch independent calls was
written into a skill, restated in a diagram, and then **failed three runs in a row** — every gap
still 33s, every call still in its own message. In the same period, four *structural* changes
landed first time and stayed landed: an op that reads its input from the run context instead of an
argument, two approvals polled in one call, and two ops recording the facts they already held
instead of waiting to be told.

The test: **would this still work if everyone involved forgot the rule?** If no, you have written
documentation, not a fix.

- **Count the failures before rewriting the instruction.** Once is a slip. Twice is a signal.
  Three times means stop writing and start restructuring.
- **Verify the behaviour changed, not that the instruction exists.** "I added it to the skill" is
  not evidence. Measure the thing the instruction was supposed to move.
- **Completeness must never rest on diligence.** A run that dropped 5 of 28 recorded facts did so
  because noting them was the agent's job to remember. The ops already held every one of those
  facts; now they record them, and the number cannot regress.

## Read the platform's current documentation, not your model of it

**Your knowledge of the system you are running on is a snapshot, and it is already out of date.**
Every step of this algorithm is capped by what you believe the platform can do: you cannot delete a
part whose cheaper built-in replacement you do not know exists, you cannot question a requirement
whose alternative you have never heard of, and step ④ will happily optimise a mechanism the
platform has since superseded.

So before the baseline: **go and read the current documentation of the platform, the tool, and the
API you are actually using.** Not the general concept of it — the version in front of you, dated.
Then baseline against what is available *now*, and pick the most efficient mechanism it offers
rather than the most familiar one.

This is not hypothetical caution. Asked how agents on a platform could communicate, an agent
answered from training data with two mechanisms and stated them as fact. The platform's own docs,
fetched minutes later, listed five — including inter-agent messaging and a shared task list that
were directly relevant, and a coordination mode close to what the system had been hand-rolling. On
the strength of the wrong list it had already recommended *against* a whole class of parallelism.

- **Prefer the platform's mechanism to your own.** A feature that ships is maintained, documented
  and usually faster than the thing you would write to replace it.
- **Date what you read.** "Checked the docs" without a date is an assumption with better manners.
- **Make this a step-0 exit test, not an intention.** An agent that only checks the docs when told
  to is depending on being told — which this skill already calls a wish rather than a fix.

## Your instruments are part of the system

The tools you measure and diagnose with are inside the scope, not outside it. Three ways they
mislead, all observed while running this algorithm rather than in anything it was pointed at:

**Instrument in ONE pass.** A measurement that costs more than the thing it measures is an
inefficiency you just introduced. Counting 32 items across 3 files as 96 separate process calls
took over two minutes and timed out; the same count in a single process took about a second. If
gathering the baseline is slow, the baseline is wrong before you read it — and you will be tempted
to sample instead of measure.

**Validate the detector before you act on its list.** Run it against one case you *know* is
positive and one you *know* is negative. A first attempt at finding unused features searched only
one of the two forms they are written in, and confidently reported as unused a feature that is used
in almost every call. Acting on that list would have deleted something load-bearing — and step ②
tells you to delete aggressively, so a bad list does damage fast.

**An N/A in the baseline is a debt, not an answer.** If a dimension cannot be measured because
nothing records it, the first fix is to make it recordable. Otherwise every future pass is blind in
exactly the same place, and the dimension stays invisible for as long as the system lives —
"failure rate per operation: N/A, nothing logs it" is a finding, not a footnote.

## Gotchas

| Gotcha | What happens | Do instead |
|--------|-------------|------------|
| One pass per step | The step ends when you get bored, not when it's empty. Left an 11.2s redundant call and 3500 tokens/run of transcription in place across two full runs. | Re-run every step until a pass yields nothing. Treat "I found something" as "not done". |
| Baseline skips a dimension | The worst inefficiency sits in the column nobody numbered, and **no later step can see it**. Machine time was measured; the agent's own effort was not, so it did not exist. | Number every dimension or write "N/A because…". Tokens and manual touches included. |
| One number covers several mechanisms | You optimize the part you *expected* to be slow. "19.6s" hid where the time actually went. | Split until one number = one mechanism. Re-measure after every change. |
| Deletion judged by silence | A `def`-to-next-`def` sweep took a module constant out with the function and passed 39 checks — a compiler cannot see a `NameError`, and the only caller ran on a rare branch. | Grep for the name, then **execute** the branch that used it. |
| Requirement judged only on its benefit | An unsatisfiable requirement charges you for every attempt and shows up as unrelated-looking bugs. One cost 196s and two silent failures per run and could never be met at all. | Cost the *attempts* and the failure rate. Ask whether it is even satisfiable here. |
| A check that cannot fail | Three "guards" were substring tests that survived their own defect — one matched the word inside its own explanatory comment. They reported safety they did not provide. | Reintroduce the defect, confirm red, restore. Name what must **happen**, not a string that may appear. |
| **A check whose input moved** | It passes on nothing and reports safety. Five guards read a directory that had been emptied — manifests complete, repo version-controlled, no app specifics — all green, all iterating an empty list. | Assert the fixture: the collection is non-empty, the file exists, the pattern matched something. And know which checks you have never mutation-tested — an unproven check is indistinguishable from a broken one. |
| **A stale claim** | Not a stale part — a stale *promise*. A gate told the developer their fix would be "recorded in xMattermost" months after that tool was retired, on the screen they read before approving. | Sweep every surface a human or an agent READS, not just the code. The human-facing one is skipped most often and is the most expensive to get wrong. |
| **A concrete value in a template** | Anything you tell an agent to copy is executable. A report template carrying one incident's real figures makes the next incident report the previous incident's numbers. | Parameterise everything inside a copy-me block. An example may stay only if it marks itself as an example. |
| **Optimising against a stale capability model** | You improve a mechanism the platform has already superseded, or hand-roll something it ships. An agent asserted two ways for agents to communicate; the current docs listed five, including the two that mattered — and it had already advised against a class of parallelism on the strength of the wrong list. | Read the platform's current docs BEFORE the baseline, and date what you read. Prefer the shipped mechanism to your own. |
| **One clock instead of two** | The per-action clock says the work is nearly optimal while most of the wall clock sits between the actions, where that clock cannot see. | Measure per action AND end-to-end for the whole task. Only the second decides whether a change was a win. |
| **The way back was assumed** | Aggressive deletion is safe only if you can undo it. A browser extension was edited across six changes on the belief that version control held a copy; it did not — the directory was inside a repo but every file in it was ignored. Being inside a repo is not being tracked by it. | Before the first edit, verify the way back covers **this file**: ask VCS whether the path is tracked, not whether a repo exists. |
| **The instrument cost more than the measurement** | Gathering the baseline as 96 separate process calls timed out; one process did it in about a second. A slow instrument pushes you to sample instead of measure. | Instrument in one pass. If collecting a number is slow, fix the collection before trusting the number. |
| **An unvalidated detector drove a deletion** | A search for unused features checked only one of the two forms they are written in, and reported a feature used in nearly every call as unused. | Validate the detector on a known-used and a known-unused case before acting on its output. |
| Nothing added back | Under-deletion reported as a clean pass. | If nothing came back you cut too little: name the next thing to cut, or the specific argument for keeping it. |
| Optimizing your way around a bad requirement | You make the wrong thing fast and it becomes permanent. | Go back to ① and delete it. Speed is not a defence for existence. |

## Step 0 — Make it work, and measure it
You cannot improve what you can't see.
- Get a **functional baseline** — it must actually work end to end. Repairs needed to *obtain* the baseline are step 0 work, not out-of-order step 3; say so and carry on.
- **Measure** what you intend to move: parts/LOC, steps, latency, cost, **tokens**, failure rate, **manual touches**. Write the numbers down.
- Every dimension gets a number or an explicit N/A. Each number covers exactly one mechanism.
- **An N/A is a debt.** If nothing records the dimension, making it recordable is the first
  fix — otherwise every later pass is blind in the same place, permanently.
- **Collect it in one pass.** An instrument slower than what it measures corrupts the
  baseline and tempts you to sample instead.
- **Run TWO clocks: per action, and end-to-end for the whole task.** They disagree, and only
  the second one decides. Measured on one system: a full set of per-action optimisations
  totalled ~25s while five avoided round trips were worth ~170s — the per-action clock said
  the work was nearly optimal, the end-to-end clock said 78% of it was overhead the
  per-action view could not see. A per-unit win that does not move the whole is not a win.
- **Exit:** every dimension has a value; no number left is an aggregate you could still
  split; both clocks are running; and you have read the current documentation for the
  platform and tools in scope, with the date, so the baseline measures what is available now.

## Step 1 — Question every requirement
The highest-leverage place to cut, because a deleted requirement deletes all the parts and work under it.
- Every requirement traces to a **named person**, never a department or "the process." Go ask that person.
- Be **most** skeptical of requirements from smart/senior people — they get challenged least, so their bad requirements survive longest.
- For each: *why does this exist? what breaks if it's gone? real problem or imagined one?*
- **And: what is it costing me to attempt?** Time spent trying, failure rate, and whether it is satisfiable at all. Repeated failure to satisfy a requirement is evidence about the requirement.
- **Exit:** a fresh pass over the surviving requirements kills none of them, and each one you kept has a named owner and a benefit that beats its attempt cost.

## Step 2 — Delete (parts, code, steps, files, config)
Delete from **first principles** — not "what does this do?" but "**does this need to exist at all?**"
- Delete the part/step/file/flag/dependency/abstraction. Delete dead code and superseded paths outright.
- **The 10% rule:** if you don't later add ~10% of what you deleted back, you didn't delete enough. Over-deletion is *required* — it is how you find the true minimum. Add back only what proves necessary.
- Delete the **requirement before the part**. Bias to removal; make others justify keeping.
- **Prove each deletion by execution**, not by silence. Grep for the name, then run the path.
- **Add-back checkpoint:** if nothing came back, name the next candidate or the argument for keeping it.
- **Exit:** removing one more thing demonstrably breaks something — shown by running it, not by inspection.

## Step 3 — Simplify what survives
Only now, on the parts that earned their place.
- Collapse duplication, merge overlapping paths, replace clever with obvious, flatten indirection.
- Two implementations of one idea is a merge, not a choice.
- **Exit:** a fresh pass finds no duplication, no dead branch and no indirection you can flatten.

## Step 4 — Optimize and accelerate
Speed up what remains — throughput, latency, round-trips, feedback loops, iteration time.
- Optimize with **data, not hunches**, and re-measure after each change. Optimizing something that should have been deleted is the classic error — that is why this is step 4.
- Batch, parallelize, overlap the independent, cut waits and hops. A blind wait longer than the check that follows it is pure latency.
- If a step is watched live, latency is UX — treat lag as a defect.
- **Exit:** a fresh pass produces no measurable gain, and every remaining cost traces to a mechanism you have named and accepted.

## Step 5 — Automate (last)
Only automate a process you have already questioned, deleted down, simplified and sped up: automating a bad process cements it.
- Automate the toil that remains: setup, checks, repetitive edits, regressions — turn each fixed bug into a lint or test.
- **Prove every check can fail.** Reintroduce its defect, confirm red, restore.
- **Automate the transcription, not the judgement.** The test: could this output be *derived* from data the system already holds? If yes, hand-authoring it is both slow and a source of divergence. Deriving one such surface deterministically immediately exposed three defects that hand-authoring had hidden, because every pass had got them wrong differently.
- **Assert each check's fixture, and track your coverage.** A guard that iterates a collection must
  first assert the collection is not empty; one that greps must assert the pattern matched
  something. Keep a list of which checks you have proven can fail — the unproven ones are where
  the rot goes, because they look identical to the proven ones.
- **Prefer the structural fix to the behavioural one** (see the section above): an op that records
  its own facts beats an instruction telling someone to record them.
- Leave judgment steps manual.
- **Exit:** every remaining manual step is judgement, and every automated check has been shown to fail without its fix.

## How to apply it (any target)
- **A skill / codebase:** baseline (LOC, files, deps, lint/tests, tokens) → question its scope → delete dead code, unused files, redundant flags → merge overlapping functions → speed up hot paths/round-trips → automate checks.
- **A system / platform:** baseline (components, steps, latency, cost) → question each component's reason to exist → delete services/steps/config → consolidate → accelerate the critical path → automate provisioning/ops.
- **A process / workflow:** baseline (steps, handoffs, cycle time, manual touches) → question each step's owner → delete steps and approvals → simplify handoffs → shorten cycle time → automate the mechanical steps.

## Corollaries (don't skip)
- **Whoever changes it must understand it hands-on** — no improving-by-proxy.
- **Move the constraint, not the comfortable part** — find the actual bottleneck and work there.
- **What you don't measure, you can't cut** — an unmeasured dimension is not a small problem, it is an invisible one.
- **Your own effort counts as system cost** — mechanical work the operator or agent repeats every run is the system's inefficiency, not theirs.
- **Beware false comradery** — being nice about a part's existence keeps dead weight alive. Challenge the work, not the person.
- **A rule that has failed twice will fail again** — the third rewrite of an instruction is a
  decision to keep the problem. Change the system instead.
- **Reversible by design, and VERIFIED so** — keep a way back (VCS, backup) so aggressive
  deletion is safe, and confirm it covers the specific files you are about to change before
  you change them. Being inside a repository is not being tracked by it; an ignored file has
  no history to return to. This is the corollary that makes ② safe, so it is the one worth
  checking rather than assuming.

## Output — the report
Deliver a phase-by-phase log with **before/after numbers**, so the win is provable. State the number of passes each step took and what the final, empty pass looked for:

```
BASELINE   parts/LOC · steps · latency · cost · TOKENS · failure-rate · MANUAL TOUCHES
           (every dimension a number or an explicit N/A; one mechanism per number)
           for each N/A: what would have to record it, and is making that the first fix?
           way back verified for the files about to change: <how>
           platform docs read, and dated: <which, when> - what did they change about the plan?
           two clocks: per-action <n> and end-to-end <n>; which one did each fix move?
① REQUIREMENTS  killed: <which, whose, and what ATTEMPTING it was costing>   passes: N
② DELETED       parts/files/steps removed (count); ~10% added back: <what>   passes: N
                each deletion verified by EXECUTING the path that used it
③ SIMPLIFIED    merges/rewrites; complexity before→after                     passes: N
④ OPTIMIZED     latency/round-trips before→after, re-measured each time      passes: N
⑤ AUTOMATED     toil removed; checks added, each PROVEN to fail without fix  passes: N
RESULT     before → after on every baseline metric
           of the fixes above, how many were STRUCTURAL vs BEHAVIOURAL, and for each
           behavioural one: what measurement shows the behaviour actually changed?
```

If you did steps out of order (optimized or automated before deleting), say so and redo — the order is the method. If any step took only one pass, say why a second pass found nothing; if you never ran a second pass, the step is unfinished.
