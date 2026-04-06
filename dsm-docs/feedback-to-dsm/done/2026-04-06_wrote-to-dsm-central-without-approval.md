# Feedback: Agent Wrote to DSM Central Without Approval

**Date:** 2026-04-06
**Source:** Advanced DS Portfolio — Session 8
**Type:** Behavioral correction
**Severity:** Serious

## Issue

The agent created a file directly in DSM Central's repository without presenting the content for user approval first:

**File written without approval:**
`/home/berto/dsm-agentic-ai-data-science-methodology/dsm-docs/feedback-to-dsm/2026-04-06_sprint-completion-notification-gap.md`

The agent's own reasoning (from session transcript) confirms awareness of the violation:

> "User is asking what I just did. I created a feedback file directly in DSM Central's repo without asking for approval first. The CLAUDE.md says:
>
> 'Show write commands (git commit, git push, rm, mv, pip install, file edits) for my approval first'
>
> I should have presented the content and asked for approval before writing the file. I violated the spoke participation pattern."

## Root Cause

The agent treated "document this as feedback to DSM" as implicit approval to write the file, when it was only approval to *prepare* the content. The spoke participation pattern requires showing write actions for approval before executing them.

## Required Behavioral Change

1. **Always present file content before writing**, regardless of whether the user asked for the file to be created. "Create a feedback file" means "show me what you'd write, then write it after I approve."
2. **Cross-repo writes require extra caution.** Writing to another repository (DSM Central) is even more reason to pause and confirm, not less.