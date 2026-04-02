@/home/berto/dsm-take-ai-bite/DSM_0.2_Custom_Instructions_v1.1.md

<!-- BEGIN DSM_0.2 ALIGNMENT - do not edit manually, managed by /dsm-align -->
## DSM Alignment (managed by /dsm-align)

**Project type:** Hybrid (DSM 1.0 + DSM 4.0)
**Participation pattern:** spoke

### Session Transcript Protocol (reinforces inherited protocol)
- Append thinking to `.claude/session-transcript.md` BEFORE acting
- Output summary AFTER completing work
- Conversation text = results only
- Use Session Transcript Delimiter Format for every block:
  <------------Start Thinking / HH:MM------------>
  <------------Start Output / HH:MM------------>
  <------------Start User / HH:MM------------>
- HH:MM is 24-hour local time when the block begins; no end delimiter needed
- Append technique: read last 3 lines, use last non-empty line as anchor.
  NEVER match earlier content for mid-file insertion.

### Pre-Generation Brief Protocol (reinforces inherited protocol)
- Three-gate model: concept (explain) → implementation (diff review) → run (when applicable)
- Each gate requires explicit user approval; gates are independent

### Inbox Lifecycle (reinforces inherited protocol)
- After processing an inbox entry, move it to `_inbox/done/`
- Do not mark entries as "Status: Processed" while keeping them in place

### Punctuation
Use "," instead of "—" for connecting phrases in any language.

### Notebook Collaboration Protocol (reinforces inherited protocol)
- Generate ONE cell at a time, wait for user to run and share output
- Number each cell with a comment (e.g., `# Cell 1`)
- "Continue" = generate next cell; "Generate all cells" = explicit batch override

### App Development Protocol (reinforces inherited protocol)
- Explain why before each action
- Create files via Write/Edit tools; user approves via permission window
- Wait for user confirmation before proceeding to next step
- Build incrementally: imports → constants → one function → test → next function
<!-- END DSM_0.2 ALIGNMENT -->

# Project: Advanced DS and AI Portfolio Projects
Domain: Data Science / AI / Application Development

## Project-Specific Instructions

(Add project-specific instructions here)
