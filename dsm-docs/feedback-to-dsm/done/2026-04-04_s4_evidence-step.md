# Feedback: Evidence Step — Quantitative Justification for What/Why/How

**Session:** 4 (2026-04-04)
**Project:** Advanced-DS-and-AI-Portfolio-Projects
**Category:** Critical Thinking extension (DSM_6.0 §1.4.2)
**Severity:** Low, enhances decision quality without blocking workflow
**Depends on:** Session 3 feedback (what/why/how universal rule)

## Problem

What/why/how (proposed in Session 3) explains *intent* (why) and *approach*
(how), but does not require the agent to ground decisions in quantitative
evidence. During Session 4, the agent presented a baseline CNN architecture
with intuitive reasoning ("3 blocks is enough", "too many params would
overfit"). The user asked: "Would there be a way to explain the selected
architecture based on hard facts and metrics?"

The intuitive reasoning was correct, but not verifiable. The user had no way
to assess whether "3 blocks" or "Dense(256)" were well-calibrated choices
without seeing the numbers.

## Root Cause

What/why/how's "how" step covers approach, constraints, and applicable rules,
but treats the choice as a qualitative decision. There is no prompt for the
agent to compute and present the quantitative basis: parameter counts,
parameter-to-sample ratios, spatial dimension progression, complexity metrics,
or benchmark references. The agent *can* produce this analysis (it did when
asked), but the protocol doesn't surface it.

## Proposed Change: Evidence (On-Demand 4th Step)

Extend what/why/how to what/why/how/evidence:

1. **What** — what is this generation? (type, structure, role in the plan)
2. **Why** — why is it needed? (what requirement or goal it serves)
3. **How** — how will it be done? (approach, constraints, applicable rules)
4. **Evidence** (on-demand) — quantitative justification for the chosen approach
   (parameter counts, ratios, spatial math, complexity metrics, benchmarks)

After presenting what/why/how, the agent asks: "Should I display the facts and
metrics to explain this approach?" The user accepts or skips. This keeps the
default flow lean while making quantitative depth available on demand.

### Why on-demand, not mandatory

Not every generation benefits from quantitative analysis. A markdown cell or a
config edit doesn't need parameter counts. Making evidence mandatory would add
friction to simple tasks. The on-demand trigger lets the user pull depth when
the decision warrants it (architecture choices, hyperparameter selection,
algorithm comparisons) and skip it otherwise.

### Example: Baseline CNN (Session 4)

**Without evidence (how only):**
> 3 conv blocks (32 → 64 → 128), GAP, Dense(256), Dropout(0.5). 3 blocks is
> enough for 224×224, 4 would add too many parameters.

**With evidence:**
> ~153K trainable parameters / 1,632 training samples = 94 params per sample.
> Rule of thumb: <10 comfortable, >50 high overfitting risk. A 4th block would
> add ~300K params, pushing ratio to ~190. Spatial progression: 224 → 112 → 56
> → 28 after 3 blocks, sufficient for GAP. This confirms the baseline will
> overfit as expected, setting a measurable floor for transfer learning.

The evidence makes the "how" auditable and teaches the user to evaluate
architecture decisions quantitatively.

### Applied to this project

Evidence step added to CLAUDE.md Section 2 (What/Why/How rule). See:
`Advanced-DS-and-AI-Portfolio-Projects/.claude/CLAUDE.md`, lines 69-90.
