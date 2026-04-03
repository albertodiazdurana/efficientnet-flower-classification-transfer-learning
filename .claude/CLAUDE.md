@/home/berto/dsm-agentic-ai-data-science-methodology/DSM_0.2_Custom_Instructions_v1.1.md

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
- Each cell is copied and pasted by the user
- Generate the content of each cell so that it can be copied with a click
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
Domain: Data Science / AI / Deep Learning

## Current Focus

Flower Classification (Oxford Flowers 102) — deep learning image classification
for a startup use case. Sprint plan: `dsm-docs/plans/sprint-1-plan.md`.

## Environment

- **Local GPU:** Quadro T1000, 4GB VRAM (CUDA 12.8)
- **Python:** 3.10.12, venv at `.venv/`
- **Framework:** TensorFlow 2.21, Keras 3.12
- **Notebook:** `notebooks/flower-classification.ipynb`
- **Kernel:** `flower_classification`

## Hardware Constraints

- EfficientNetB0 at 224×224, batch 16 fits locally
- EfficientNetB3+ or 300×300 resolution may OOM, fall back to Colab
- Monitor VRAM with `nvidia-smi` before training runs

## Dataset Notes (Oxford Flowers 102)

- TFDS `oxford_flowers102`: train (1,020), val (1,020), test (6,149)
- Training set is tiny (10/class), must merge train+val for training
- EfficientNet expects [0,255] input, use `preprocess_input` (NOT [0,1])
- No vertical flips (flowers don't appear upside-down)
- Label smoothing 0.1 for fine-grained classification

## Portfolio Standards

- Notebook must be Colab-compatible for reviewers (one-click run)
- Single notebook tells the full story: EDA → baseline → transfer learning → evaluation
- Clear markdown section headers separate phases
- Business context and interpretation in every section
