# Feedback: Figure Validation — Agent Reads Generated Images

**Session:** 4 (2026-04-04)
**Project:** Advanced-DS-and-AI-Portfolio-Projects
**Category:** Notebook collaboration gap
**Severity:** Low, improves analysis quality without blocking workflow

## Problem

In the notebook collaboration protocol, the agent generates code that produces
plots and figures, but never sees the actual output. The user runs the cell,
views the figure in the notebook, and the agent proceeds based only on printed
metrics or the user's description. The agent is effectively blind to its own
visual output.

This matters because:
- Plots can reveal anomalies that metrics alone miss (unexpected curves, visual
  artifacts, mislabeled axes, color issues)
- The agent cannot validate whether the figure matches its expectations without
  seeing it
- The user bears the full burden of visual quality control

## Root Cause

The notebook collaboration protocol (copy-paste cells, share output) was
designed around text output. Figures are generated and saved to disk, but the
protocol has no step for the agent to read them back. Since multimodal LLMs
can read images via their Read tool, this is a capability gap in the protocol,
not a technical limitation.

## Proposed Change: Figure Validation Rule

Add to the Notebook Development Protocol:

> When a cell generates figures, save them to `outputs/figures/` and after
> execution the agent reads the saved image to explore results and validate
> the analysis before proceeding.

This closes the feedback loop: the agent generates the plot code, the user
runs it, the agent reads the saved figure, validates the output, and then
proceeds with informed next steps.

### Why save-then-read, not inline

Notebook outputs (base64-encoded images in `.ipynb` JSON) are not reliably
readable by the agent. Saving to a file path and using the Read tool on the
image file is the reliable path for multimodal agents to consume visual output.
The `outputs/figures/` convention also creates a persistent artifact trail.

### Applied to this project

Added as rule 6 in the Notebook Development Protocol, CLAUDE.md Section 3.
See: `Advanced-DS-and-AI-Portfolio-Projects/.claude/CLAUDE.md`.
