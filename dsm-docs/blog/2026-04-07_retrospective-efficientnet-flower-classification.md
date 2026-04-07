# Retrospective: EfficientNet Flower Classification (Transfer Learning)

**Project finalized:** 2026-04-07
**Sessions:** 11
**Final result:** 91.90% test accuracy (TTA) on Oxford Flowers 102

## The story in one paragraph

Built a 102-class flower species classifier using only about 10 labeled images per class. A from-scratch CNN managed 24.56%, barely above random. EfficientNetB0 transfer learning with progressive unfreezing and test-time augmentation reached 91.90%, a 3.7x improvement, in a single Colab-compatible notebook that runs end-to-end in under 10 minutes on a T4.

## Three lessons worth remembering

### 1. Checkpoint before every risky fine-tuning phase

Phase 4b (top 30% unfrozen) was the best weights the project ever produced. Phase 4c (full fine-tune) degraded them every time, even at LR 1e-6. On a dataset with ~1700 training images, the ImageNet feature statistics in early layers were more valuable frozen than adapted. Without explicit weight checkpoints before each phase, the best model was unrecoverable. The fix, added in Session 8: save weights and training histories as JSON after each phase, auto-detect and reload on rerun, and auto-rollback 4c if val accuracy regresses.

### 2. Augmentation is not always help

A controlled comparison showed data augmentation hurt validation accuracy by ~5pp during head-only training with a frozen backbone. Ten epochs wasn't enough time for the model to benefit from the harder augmented examples. Augmentation started paying off only once the backbone was unfrozen and had real learning capacity. The takeaway: augmentation interacts with training budget and trainable capacity; measure, don't assume.

### 3. TTA is free accuracy

Five-view test-time augmentation (original, horizontal flip, ±90 degree rotation, center crop) added +0.89pp accuracy and +0.01 macro F1 for 1.1 minutes of extra inference. No retraining. Implementation note: run each augmentation as one bulk `model.predict()` over the full test set, not per-image. The first implementation did 6,149 individual predicts and was ~30x slower.

## Seed for a full blog post

Outline:
- Setup: 102 classes, 10 images each, why this is brutal
- Baseline: from-scratch CNN at 24% (near-random) and why
- Transfer learning phases: head-only, partial unfreeze, failed full fine-tune
- The checkpoint recovery that turned a disappointment into a deliverable
- Augmentation experiment: a counterintuitive result
- TTA: the one-minute accuracy boost
- Closing: how far transfer learning gets you with almost no data
