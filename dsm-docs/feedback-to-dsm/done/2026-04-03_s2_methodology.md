# Feedback: CLAUDE.md Setup Protocol Incomplete

**Session:** 2 (2026-04-03)
**Project:** Advanced-DS-and-AI-Portfolio-Projects
**Category:** Methodology gap
**Severity:** High — blocks correct project initialization

## Problem

`/dsm-go` only generates Section 1 (DSM_0.2 ALIGNMENT) in CLAUDE.md. Sections 2–4 have no defined protocol, so they were either missing or incomplete. The user had to manually request and correct them. This caused friction at project start and risks inconsistent CLAUDE.md across spoke projects.

## Proposed Change: 4 Required CLAUDE.md Sections

Every project CLAUDE.md must contain these 4 sections before implementation begins (hard gate):

| # | Section | Content |
|---|---------|---------|
| 1 | **DSM_0.2 ALIGNMENT** | DSM alignment block, managed by `/dsm-align` |
| 2 | **Participation pattern** | Instructions specific to participation pattern (spoke, hub, standalone, contributor, private, dsm-ecosystem, other?) |
| 3 | **Project type** | Instructions specific to project type (notebook, hybrid, documentation, app) |
| 4 | **Project specific** | Project structure, objectives, tech requirements, domain constraints |

## When Each Section Is Added

1. **DSM_0.2 ALIGNMENT** — At first `/dsm-go`. Already works.
2. **Participation pattern** — After `/dsm-go` completes and the initial idea is framed. Trigger: references, notes, or materials from any source folder (`_references/`, external folders) are read, enabling a preliminary plan in `dsm-docs/plans/`. Once a preliminary plan exists, the participation pattern is understood and added to CLAUDE.md. Requires user approval.
3. **Project type** — Once the preliminary plan exists, the project type is understood and added to CLAUDE.md. Independent from Section 2, requires separate user approval.
4. **Project specific** — After the preliminary plan, research grounds it on best practices and state of the art (results in `dsm-docs/research/`). After drafting the first plan in `dsm-docs/plans/`, project-specific instructions are added to CLAUDE.md. Requires user approval.

## How the Agent Manages This

- **Empty projects or new folders:** Only accept `/dsm-go`. Running other DSM commands could damage project initiation.
- **After `/dsm-go` completes:** The agent lists the 4 tasks needed to complete CLAUDE.md.
- **After each task completes:** The agent reports remaining tasks and suggests the next one.
- **Hard gate:** Implementation must not start until all 4 sections are present in CLAUDE.md. The agent enforces this by checking CLAUDE.md completeness before allowing implementation work.
- **Each section requires explicit user approval** before being written to CLAUDE.md.
