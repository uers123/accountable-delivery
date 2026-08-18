# accountable-delivery

A Claude Code **metaskill** that enforces a mandatory closed loop — **self-test → reflect → rework → pass quality gates** — before any output is delivered. No testing, no delivery. No passing gates, no submission. No reflection, no growth.

> Core principle: *I am accountable for this deliverable.*

## Why this skill exists

Large language models are excellent at producing confident output — and notoriously bad at admitting when that output is wrong. Untested code gets shipped, edge cases get missed, and "it works" is asserted rather than verified.

`accountable-delivery` turns delivery into a **behavioral contract**. Instead of asking the model to *try* to be careful, it requires a structured pipeline the model cannot skip: test its own work, analyze the root cause of every failure, rework until every selected quality gate passes, and report an honest confidence level.

## How it works: a five-stage closed loop

```
Stage 0  Commitment declaration
   →  Stage 1  Requirements confirmation & quality-gate definition
   →  Stage 2  Implementation & self-testing (tests run and logged, pass/fail)
   →  Stage 3  Structured reflection & root-cause analysis     ← the core
   →  Stage 4  Rework & verification (re-run all tests; loop to Stage 3 on failure)
   →  Stage 5  Delivery & summary (gate checklist, lessons, confidence)
```

**The loop is complete only when every selected quality gate passes.** The model then reports what it delivered, how many rework cycles it took, what it learned, and — honestly — how confident it is.

## Installation

Claude Code discovers skills from the `skills` directory. To install for all your projects:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/uers123/accountable-delivery.git ~/.claude/skills/accountable-delivery
```

To share it within a single project, place the folder at `.claude/skills/accountable-delivery/` in that project's repository. The only required file is `SKILL.md`; `references/quality-gates.md` is picked up automatically when the skill selects quality gates.

> Skill discovery paths can differ across Claude Code versions — when in doubt, put the folder in a `skills` directory and confirm Claude lists it.

## Usage

The skill activates automatically when a task matches its description — starting implementation, fixing bugs, or being asked to "deliver" or "complete" work. To force it, ask explicitly:

> "Deliver this following the accountable-delivery skill."

From then on, expect to see:

1. A **commitment declaration** before any work starts.
2. A restated requirement plus the chosen quality gates and their pass criteria.
3. A **test log** with real pass/fail results — no fake passes.
4. A **structured reflection log** (error classification, root-cause analysis, lesson learned) whenever a test fails.
5. A **delivery summary report** ending with an explicit confidence level and a deliver / do-not-deliver decision.

You can tune when the skill triggers by editing the trigger phrases in the `description` field of `SKILL.md`'s frontmatter.

## Quality gates

**Hard gates — every task must pass all four:**

| Gate | What it verifies |
|------|------------------|
| Q1 Functional completeness | Every requested feature is implemented and has a test |
| Q2 Result correctness | Output matches expectations; all assertions pass |
| Q3 Exception handling | Bad input never crashes; errors are clear and meaningful |
| Q4 No regression | Existing behavior keeps working |

**Recommended gates — pick per task type:** Q5 performance baseline, Q6 code readability, Q7 input validation, Q8 security, Q9 compatibility. `references/quality-gates.md` defines the pass criteria for every gate and maps task types to gate sets (e.g. API development → Q1–Q4 + Q6, plus Q7/Q8; bug fixes → Q2–Q4 + Q6).

## Failure taxonomy

When a test fails, the model must classify it as exactly one of four types:

- **Requirement misunderstanding** — what was built is not what was asked for
- **Technical implementation error** — the goal was right, the implementation was wrong
- **Boundary omission** — an input or scenario was never considered
- **Performance / quality shortfall** — functionally right, but not good enough

Classification feeds root-cause analysis, so rework fixes the *cause*, not just the symptom.

## Key rules

1. Never skip reflection (Stage 3) — reworking without reflecting is patching, not improving.
2. No fake passes — tests must actually run and their results be verified.
3. Lessons must be recorded — an unrecorded lesson is no evolution.
4. Quality gates are non-negotiable — a failing gate means no delivery.
5. Report confidence honestly — if unsure, say "low" and explain why.

## Known limitations

- **It depends on model self-discipline.** The model executes the loop itself, so the contract is only as strong as the model's instruction-following. Treat it as a behavioral contract, not a correctness or security guarantee.
- **Self-verification is only as good as the tests.** Where real test runners, linters, or CI exist, run this skill *alongside* them and let external tools confirm the model's claims.
- **It adds overhead.** Structured reports lengthen every task; for trivial one-liners you may prefer a lighter workflow.
- **Depth varies by model.** Weaker models may produce shallower reflections or less reliable self-tests.

## License

MIT © 2026 uers123
