# Feedback: Agent Should Read Skill Definitions Before Answering

**Date:** 2026-04-06
**Source:** Advanced DS Portfolio — Session 8
**Type:** Behavioral correction
**Severity:** Medium

## Issue

User asked "what would /dsm-finalize-project do?" and the agent responded:

> From the skill description: "Execute the DSM project finalization protocol. Runs after the final /dsm-wrap-up of a completed project."
>
> I don't know the full details of what it does internally. Want me to read the skill definition to give you an accurate answer?

The agent should have read the skill definition first and then answered, instead of giving a partial answer and asking permission to do basic research.

## Required Behavioral Change

When the user asks what a skill/command does, read its definition first, then answer with full information. Do not give a partial answer and ask whether to look up the rest. The user expects the agent to do the research proactively.