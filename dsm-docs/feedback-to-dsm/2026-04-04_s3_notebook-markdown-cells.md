# Feedback: What/Why/How — Universal Pre-Generation Reasoning

**Session:** 3 (2026-04-04)
**Project:** Advanced-DS-and-AI-Portfolio-Projects
**Category:** Agent behavior gap → universal pattern proposal
**Severity:** Medium, degrades output quality across all generation types

## Problem

When transitioning from Phase 1 (EDA) to Phase 2 (Data Pipeline) in Session 2,
the agent generated Cell 8 as code directly, skipping the markdown header cell
that introduces the new phase. The user had to explicitly request a markdown cell.

## Root Cause

Three existing rules already required the markdown cell:

1. **Portfolio Standards** ("Clear markdown section headers separate phases"):
   Phase 2 is a new phase. A markdown cell was mandatory.
2. **Notebook Protocol** ("unless the first cell contains markdown only, then
   generate up to TWO cells"): This rule is designed for section boundaries,
   generate markdown + code as a pair.
3. **Session 1 precedent**: Phase 1 used markdown cells (Cells 1, 3, 5) to
   introduce sections. The pattern was established but broken at Phase 2.

All three rules were available in context (loaded via CLAUDE.md at session
start). The agent did not need to read them separately. The failure was that
the agent's pre-generation reasoning was purely content-focused ("what code
goes in Cell 8?") and skipped the structural decision ("what cell type is
needed first?"). The thinking step (Session Transcript Protocol lines 57-66
in Session 2) went straight from "Cell 7 exists → Phase 2 → data pipeline
code" without checking structural requirements for a phase transition.

This is a reasoning process gap, not an information access gap. The
thinking-before-acting step should have included a structural checkpoint,
but the agent treated cell generation as only a content decision.

## Proposed Change: What/Why/How Universal Rule

The initial fix was a notebook-specific Cell Generation Pre-Flight checklist.
Root cause analysis revealed the deeper issue: the agent's reasoning before
ANY generation lacks structure, not just notebook cells. The same failure
pattern (skipping structural checks, jumping to content) can occur when
generating code files, documentation, or any other artifact.

### Universal rule (participation-pattern level)

Before generating any artifact (cell, code, file, text), apply Critical
Thinking (DSM_6.0 §1.4.2) by answering:

1. **What** — what is this generation? (type, structure, role in the plan)
2. **Why** — why is it needed? (what requirement or goal it serves)
3. **How** — how will it be done? (approach, constraints, applicable rules)

Present what/why/how to the user. Wait for approval before generating.

Universal interaction pattern:
- Agent applies what/why/how → presents to user → user reviews and approves →
  agent generates → user confirms or corrects → next step

This replaces the need for each project type (notebook, app, documentation)
to define its own interaction pattern. They all inherit the universal pattern
and add only context-specific checks.

### Notebook-specific extension: Cell Pre-Flight

In addition to what/why/how, notebook cell generation adds:
```
Pre-flight for Cell N:
- Phase/section: [new or continuation?]
- If new phase: markdown cell first (use two-cell allowance)
- Cell type: [markdown / code]
```

### Why this works

The thinking-before-acting step already exists (Session Transcript Protocol),
but without structure it gravitates toward content reasoning and skips
structural checks. What/why/how gives the thinking step a universal template.
The "how" question forces the agent to check applicable rules from CLAUDE.md
before generating, which is exactly the step that was skipped in the original
Cell 8 incident.

### Applied to this project

- Universal what/why/how rule added to CLAUDE.md Section 2 (Participation
  Pattern)
- Notebook Cell Pre-Flight in Section 3 references the universal rule
- App Development interaction pattern in Section 3 references the universal
  rule
