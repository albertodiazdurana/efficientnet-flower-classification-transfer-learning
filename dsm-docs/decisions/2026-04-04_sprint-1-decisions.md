# Sprint 1 Decision Log

**Sprint:** 1 — EDA, Baseline & Transfer Learning
**Date range:** 2026-04-03 to 2026-04-04
**Sessions:** 1-6

---

### DEC-001: Merge train and validation splits

**Context:** Oxford Flowers 102 has only 10 images per class in both train and val (1,020 each).
**Decision:** Merge into 2,040 images, then split 80/20 for training/validation.
**Rationale:** 10 images per class is insufficient for training. Merging doubles the training data. The test set (6,149 images) remains untouched for final evaluation.

### DEC-002: EfficientNetB0 as primary backbone

**Context:** Research compared EfficientNetB0, EfficientNetB3, and ResNet50.
**Decision:** Use EfficientNetB0 at 224x224 for local training.
**Rationale:** EfficientNet outperforms ResNet50 by 1-3% on fine-grained tasks per literature. B0 fits within 4GB VRAM (Quadro T1000). B3 at 300x300 deferred as stretch goal.

### DEC-003: 80/20 train/val split (changed from planned 85/15)

**Context:** Sprint plan specified 85/15 split.
**Decision:** Used 80/20 instead.
**Rationale:** Simpler ratio. With 2,040 samples, the difference is ~100 images (1,632 vs 1,734 train). Validation set of 408 images provides stable metrics.

### DEC-004: Progressive unfreezing (3-phase fine-tuning)

**Context:** Transfer learning strategy for small datasets.
**Decision:** Phase 4a (head only, lr=1e-3) → Phase 4b (top 30%, lr=1e-4 cosine) → Phase 4c (full, lr=1e-5 cosine).
**Rationale:** Standard approach, avoids catastrophic forgetting. Each phase trains fewer epochs with lower learning rates.

### DEC-005: Accept Phase 4c regression, do not re-run

**Context:** Phase 4b achieved 93.87% val accuracy. Phase 4c (full fine-tune) degraded to 90.93% (-2.94pp). Weights were not checkpointed before 4c.
**Decision:** Accept 89.27% test accuracy (from 4c weights) and document as a lesson.
**Rationale:** Re-running would require repeating 4a+4b (~6 min). The regression demonstrates a real risk of full fine-tuning on small datasets, which is educational for the portfolio. Lesson: always checkpoint before aggressive fine-tuning.

### DEC-006: Remove ReduceLROnPlateau from cosine decay phases

**Context:** Phases 4b and 4c use CosineDecay LR schedules. ReduceLROnPlateau was included in callbacks.
**Decision:** Removed ReduceLROnPlateau from 4b (Colab fix) and 4c (during session 5).
**Rationale:** ReduceLROnPlateau modifies the optimizer's LR, which conflicts with schedule-based LR. The schedule already handles decay. Keeping both causes unpredictable LR behavior.