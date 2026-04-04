# Feedback: Agent must cross-reference sprint plan before declaring completion

**Date:** 2026-04-04
**Session:** 6
**Type:** Methodology Observation
**Priority:** High
**Source:** User correction during session 6

## Observation

After completing the final notebook cells (comparison chart, confused pairs, conclusions),
the agent declared "Sprint 1 notebook is complete" and suggested wrapping up. This was
incorrect: the sprint plan had open SHOULD deliverables (augmentation comparison), an unmet
experiment gate (89.27% < 90% target), and an incomplete sprint boundary checklist (README,
decision log, blog entry).

## Root Cause

The agent relied on the checkpoint's "What Remains" list (which only tracked notebook cells)
instead of reading the sprint plan to verify all deliverables, success criteria, and checklists.

## Impact

The user was led to believe the project was ready for finalization, and began considering
`/dsm-finalize-project` when significant work remained.

## Recommendation

Add to DSM_0.2 or agent instructions: after completing a work block, the agent MUST read the
sprint plan and cross-reference completed vs remaining deliverables before suggesting wrap-up
or next steps. The checkpoint is a session-level artifact, not a sprint-level completion tracker.

## Affected DSM Section

- DSM_0.2 Module A (Session Transcript Protocol / wrap-up behavior)
- Sprint plan protocol (plan as authoritative source of truth for completion)