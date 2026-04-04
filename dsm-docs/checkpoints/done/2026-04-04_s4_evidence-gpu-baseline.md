**Consumed at:** Session 5 start (2026-04-04)

# Checkpoint — Session 4: Evidence Step, GPU Setup, Baseline CNN

**Date:** 2026-04-04
**Session:** 4
**Branch:** session-1/2026-04-03
**Commit:** 1330936
**Type:** Lightweight wrap-up

---

## What Was Done

- Added **evidence** (on-demand) as 4th step to what/why/how rule in CLAUDE.md
- Added **figure validation** rule to notebook protocol (rule 6: save + read images)
- DSM feedback written: evidence-step, figure-validation, cuda-setup-pitfalls
- Resolved GPU/CUDA setup (4 pitfalls):
  1. pip nvidia packages with `--only-binary :all:` and pinned versions
  2. nvJitLink camelCase symlink fix
  3. VS Code Jupyter ignores kernel.json env field
  4. ctypes.CDLL preload + `tf.constant(0)` lazy init + `TF_FORCE_GPU_ALLOW_GROWTH`
- Created central GPU guide: `~/gpu-setup.md`
- Cell 1 updated with GPU preload fix (Colab-compatible)
- Cell 10: Phase 3 markdown (Baseline CNN introduction)
- Cell 11: Baseline CNN trained — 22.8% val accuracy, 153K params, 3.8 min on GPU
- Cell 12: Training curves code generated (not yet run)

## What Remains (Sprint 1)

- [ ] Rerun all cells with GPU (Cell 1 fix applied, needs full rerun)
- [ ] Cell 12: Training curves (code ready, run and validate figure)
- [ ] Cell 13: Test set evaluation (accuracy, per-class metrics, confusion matrix)
- [ ] Phase 4: Transfer learning (EfficientNetB0, progressive fine-tuning)

## Key Decisions

- Evidence step is on-demand, not mandatory (keeps default flow lean)
- GPU setup via pip packages + ctypes preload (not system CUDA toolkit)
- Baseline CNN: 3 conv blocks, 153K params, intentionally underfits to motivate transfer learning

**Deferred to next full session:**
- [ ] Inbox check
- [ ] Version check
- [ ] Reasoning lessons extraction
- [ ] Feedback push
- [ ] Full MEMORY.md update
- [ ] README change notification check
- [ ] Contributor profile check