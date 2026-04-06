# Sprint 1 Decision Log

**Sprint:** 1 — EDA, Baseline & Transfer Learning
**Date range:** 2026-04-03 to 2026-04-06
**Sessions:** 1-8

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

### DEC-007: Automatic 4c rollback with weight checkpointing

**Context:** Session 8 re-ran 4a+4b+4c with weight saving after each phase. 4c degraded again (92.16% → 88.97%, -3.19pp) even with 10x gentler LR (1e-6 vs 1e-5).
**Decision:** Cell 19 saves 4b weights before 4c, automatically restores if 4c degrades. 4c weights file stores the rolled-back state to skip retraining on re-runs.
**Rationale:** Full fine-tuning consistently hurts on this dataset size. The rollback makes the notebook safe to run end-to-end without manual intervention. Supersedes DEC-005.

### DEC-008: Keep augmentation despite head-only penalty

**Context:** Controlled experiment showed augmentation reduces val accuracy by -4.90pp at head-only stage (frozen backbone, 10 epochs).
**Decision:** Keep augmentation in the training pipeline for all phases.
**Rationale:** The penalty only appears when the backbone is frozen and cannot adapt to augmented inputs. During Phase 4b (partial unfreeze), augmentation provides the variety needed to generalize. Removing it would optimize for 4a at the expense of 4b.

### DEC-009: TTA with 5 views (no vertical transforms)

**Context:** Chose TTA views for inference-time accuracy boost.
**Decision:** 5 views: original, horizontal flip, +90°, -90°, center crop (85%).
**Rationale:** Flowers have natural upright orientation, so vertical flips are excluded (consistent with training augmentation). 90° rotations help with varied camera angles. Center crop focuses on the flower subject. Result: +0.89pp accuracy for 1.1 min inference cost.