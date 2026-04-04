**Consumed at:** Session 6 start (2026-04-04)

# Checkpoint — Session 5: Transfer Learning Complete

**Date:** 2026-04-04
**Session:** 5
**Branch:** session-1/2026-04-03
**Commit:** 32f74bf
**Type:** Lightweight wrap-up

---

## What Was Done

- Cell 12: Baseline training curves (rerun with GPU, 29.17% val accuracy)
- Cell 13: Baseline test evaluation (24.56% test accuracy, macro F1 0.1984)
- Cell 14: Phase 4 markdown (transfer learning intro, progressive unfreezing strategy)
- Cell 15: Phase 4a head training (88.97% val, 2.3 min)
- Cell 16: Phase 4b partial unfreeze top 30% (93.87% val, 3.4 min)
- Cell 17: Phase 4c full fine-tune (90.93% val, -2.94pp regression, 4.2 min)
- Cell 18: Markdown documenting 4c regression and checkpoint lesson
- Cell 19: Transfer learning training curves (all 3 phases, best epoch 20 in 4b)
- Cell 20: Transfer model test evaluation (89.27% test accuracy, macro F1 0.8914)
- Fixed CLAUDE.md figure validation rule (path-agnostic)
- Fixed ReduceLROnPlateau + CosineDecay incompatibility

## What Remains (Sprint 1)

- [ ] Cell 21: Side-by-side comparison (baseline vs transfer, metrics + visual)
- [ ] Cell 22: Most-confused pairs analysis (SHOULD deliverable)
- [ ] Phase 5: Final evaluation summary and business recommendations
- [ ] Consider re-running 4a+4b (skip 4c) to recover 93.87% weights (optional)

## Key Decisions

- Accepted 4c regression (90.93%) instead of re-running 4b, documented as learning moment
- Phase 4c lesson: always save model checkpoint before risky fine-tuning phases
- ReduceLROnPlateau incompatible with LR schedules (CosineDecay), removed from callbacks

## Lessons Learned

- `restore_best_weights` only restores within the current `.fit()` call, not to pre-fit state
- Recompiling resets optimizer state (Adam momentum/velocity), causing instability
- Full fine-tune on 1,734 samples with 4.2M params risks overfitting even at lr=1e-5

**Deferred to next full session:**
- [ ] Inbox check
- [ ] Version check
- [ ] Reasoning lessons extraction
- [ ] Feedback push
- [ ] Full MEMORY.md update
- [ ] README change notification check
- [ ] Contributor profile check