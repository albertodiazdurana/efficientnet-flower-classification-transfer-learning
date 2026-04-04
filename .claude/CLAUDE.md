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


## Section 2: Participation Pattern (spoke)

### Working Style
- Confirm understanding before proceeding
- Be concise in answers
- Do not generate files before providing description and receiving approval

### Command Execution
- Execute read-only commands (git status, ls, cat, grep, find, python -c for reading) without asking
- Show write commands (git commit, git push, rm, mv, pip install, file edits) for my approval first

### Plan Mode Protocol
Before implementing any significant feature or change:
1. Thoroughly explore the codebase to understand existing patterns
2. Identify similar features and architectural approaches
3. Consider multiple approaches and their trade-offs
4. Ask clarifying questions if approach is unclear
5. Design a concrete implementation strategy
6. Present plan for user approval before writing/editing any files

This is a read-only exploration and planning phase - DO NOT write or edit files until plan is approved.

### What/Why/How (Critical Thinking before generation)

Before generating any artifact (cell, code, file, text), apply Critical
Thinking (DSM_6.0 §1.4.2):
1. **What** — what is this generation? (type, structure, role in the plan)
2. **Why** — why is it needed? (what requirement or goal it serves)
3. **How** — how will it be done? (approach, constraints, applicable rules)
4. **Evidence** (on-demand) — quantitative justification for the chosen approach
   (parameter counts, ratios, spatial math, complexity metrics, benchmarks).
   After presenting what/why/how, ask: "Should I display the facts and metrics
   to explain this approach?" Generate only if the user says yes.

Present what/why/how to the user. Wait for approval before generating.

Universal interaction pattern:
- Agent applies what/why/how → presents to user → user reviews and approves →
  agent generates → user confirms or corrects → next step
- After what/why/how approval, offer evidence: "Should I display the facts and
  metrics to explain this approach?" → user accepts or skips
- "Continue" or "yes" = approval to generate
- "Explain more" = deeper what/why/how before proceeding

Project-type protocols (Notebook, App) add context-specific checks on top of
this pattern.


## Section 3: Project Type (hybrid: notebook + app)

### Code Output Standards
- Print statements show actual values (shapes, metrics, counts)
- Avoid generic confirmations: "Complete!", "Done!", "Success!"
- Let results speak: Show df.shape, not "Data loaded successfully!"

### Notebook Development Protocol
When generating notebook cells:
1. Generate ONE cell at a time; unless the first cell contains markdown only, then generate up to TWO cells
2. Wait for user approval OR execution output before generating next cell
3. Never generate multiple cells without explicit request
4. Adapt each cell based on actual output from previous cells
5. Number each cell with a comment (e.g., `# Cell 1`, `# Cell 2`) for easy reference in discussions
6. When a cell generates figures, after execution the agent reads the saved
   image (using the path printed in cell output) to explore results and
   validate the analysis before proceeding

**Cell Generation Pre-Flight:** In addition to what/why/how, include these
notebook-specific checks in the Session Transcript thinking block:
```
Pre-flight for Cell N:
- Phase/section: [new or continuation?]
- If new phase: markdown cell first (use two-cell allowance)
- Cell type: [markdown / code]
```

Interaction pattern: Follows what/why/how (Section 2). Additionally:
- "Generate all cells" = explicit batch override

### App Development Protocol

When building application code (packages, modules, scripts):
1. Guide step by step through the development process
2. Explain **why** before each action
3. Provide code segments for user to copy/paste
4. Wait for user confirmation before proceeding to next step
5. Generate no files directly - user creates all artifacts
6. Build modules incrementally: imports → constants → one function → test → next function
7. Use Test-Driven Development (TDD): write tests in `tests/` alongside code

**Interaction pattern:** Follows what/why/how (Section 2). Additionally:
- "Done" or "next" = proceed to next step

Why: This establishes a learning-focused protocol where you remain in control of file creation, ensuring you understand each piece as it's built. Different from the notebook protocol which is about incremental cell generation.


## Section 4: Project Specific

### Author
**Alberto Diaz Durana**
[GitHub](https://github.com/albertodiazdurana) | [LinkedIn](https://www.linkedin.com/in/albertodiazdurana/)

### Project: Advanced DS and AI Portfolio Projects
Domain: Data Science / AI / Deep Learning

### Current Focus

Flower Classification (Oxford Flowers 102), deep learning image classification
for a startup use case. Sprint plan: `dsm-docs/plans/sprint-1-plan.md`.

### Environment

- **Local GPU:** Quadro T1000, 4GB VRAM (CUDA 12.8)
- **Python:** 3.10.12, venv at `.venv/`
- **Framework:** TensorFlow 2.21, Keras 3.12
- **Notebook:** `notebooks/flower-classification.ipynb`
- **Kernel:** `flower_classification`

### Hardware Constraints

- EfficientNetB0 at 224×224, batch 16 fits locally
- EfficientNetB3+ or 300×300 resolution may OOM, fall back to Colab
- Monitor VRAM with `nvidia-smi` before training runs

### Dataset Notes (Oxford Flowers 102)

- TFDS `oxford_flowers102`: train (1,020), val (1,020), test (6,149)
- Training set is tiny (10/class), must merge train+val for training
- EfficientNet expects [0,255] input; Keras 3 normalizes internally (`preprocess_input` is a no-op)
- No vertical flips (flowers don't appear upside-down)
- Label smoothing 0.1 for fine-grained classification

### Portfolio Standards

- Notebook must be Colab-compatible for reviewers (one-click run)
- Single notebook tells the full story: EDA → baseline → transfer learning → evaluation
- Clear markdown section headers separate phases
- Business context and interpretation in every section
