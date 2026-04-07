# Sprint 1: EDA, Baseline & Transfer Learning

> **Status (2026-04-07): Project finalized.** Sprint 1 closed at 91.90% test accuracy (TTA, gate was 90%). **Sprint 2 will not take place.** This project is archived as a portfolio deliverable; no further sprints are planned.

**Goal:** Deliver a trained transfer-learning model on Oxford Flowers 102 that exceeds 90% test accuracy, with documented EDA and baseline comparison.
**Prerequisites:** Local Python environment with TensorFlow, TFDS accessible
**Development environment:** Local development and training (Quadro T1000 4GB VRAM). Notebook kept Colab-compatible for portfolio reviewers. Fall back to Colab for larger backbones (B3+) if local VRAM insufficient.
**Notebook location:** `notebooks/flower-classification.ipynb`

## Research Assessment

Research complete (2026-04-03). Key findings integrated into this plan:
- Training set is tiny: 10 images/class (1,020 total). Must merge train+val (2,040).
- EfficientNetB3 outperforms ResNet50 by 1-3% on fine-grained tasks.
- 300×300 resolution captures petal detail better than 224×224.
- EfficientNet expects [0,255] input, not [0,1]. Must use `preprocess_input`.
- Label smoothing (0.1) and small batch sizes (16-32) recommended.
- Vertical flips hurt; color augmentation helps.
- Realistic target: 95-97% with good fine-tuning.

## Experiment Gate

- [ ] **Capability sprint:** flower classification model with success criteria
  defined below. Test data = TFDS original test split (6,149 images).
  Expected output: per-class predictions with ≥ 90% overall accuracy.

## Branch Strategy

Sprint branch: `sprint-1/eda-baseline-transfer` off `main`.

---

## Deliverables

### MUST (Sprint fails without these)
- [ ] EDA notebook -- class distribution, sample visualization, dataset structure analysis
- [ ] Data pipeline -- TFDS loading, train+val merge, stratified split, preprocessing, augmentation
- [ ] Baseline CNN -- simple model establishing performance floor
- [ ] Transfer learning model -- EfficientNetB0 with progressive fine-tuning, ≥ 90% test accuracy
- [ ] Evaluation report -- confusion matrix, classification report, per-class metrics

### SHOULD (Expected, defer if blocked)
- [ ] Augmentation comparison -- document impact of augmentation on val accuracy
- [ ] Training curves -- loss/accuracy plots for all models (baseline vs transfer)
- [ ] Most-confused pairs analysis -- identify and visualize top-10 confused class pairs

### COULD (Stretch goals)
- [ ] Scale-up experiment -- EfficientNetB3 at 300×300 (Colab if OOM locally)
- [ ] Test-time augmentation (TTA) -- measure accuracy boost from TTA
- [ ] Business recommendations section -- deployment format, mobile optimization notes

---

## Phases

### Phase 1: EDA & Data Understanding
- **Focus:** Load dataset, understand structure, document findings
- **Deliverables:** EDA notebook
- **Execution mode:** notebook
- **DSM references:** DSM 1.0 (Notebook Collaboration Protocol)
- **Success criteria:** EDA notebook complete with class distribution plot, sample grid per class, dataset size summary, identified visual similarity clusters
- **Activities:**
  1. Load Oxford Flowers 102 via TFDS, inspect all splits (train/val/test)
  2. Document split sizes: train (1,020), val (1,020), test (6,149)
  3. Plot class distribution across all splits
  4. Display 3-5 sample images for 10+ classes, annotate visual similarities
  5. Analyze and document image size distribution before resizing
  6. Identify potential difficult class pairs (similar colors/shapes)

### Phase 2: Data Pipeline
- **Focus:** Build robust preprocessing and augmentation pipeline
- **Deliverables:** Data pipeline
- **Execution mode:** notebook
- **DSM references:** DSM 1.0 (Notebook Collaboration Protocol)
- **Success criteria:** Pipeline produces correctly shaped, preprocessed batches; augmentation visually verified
- **Activities:**
  1. Merge train + val splits into combined training set (2,040 images)
  2. Create stratified 85/15 train/val split from combined set
  3. Build preprocessing function:
     - Resize to 224×224 (B0 default; 300×300 for B3 stretch)
     - Apply `tf.keras.applications.efficientnet.preprocess_input` (keeps [0,255] range)
  4. Build augmentation layer:
     - Random horizontal flip (no vertical)
     - Random rotation (±20°)
     - Random zoom (±15%)
     - Random brightness/contrast adjustment
  5. Configure `tf.data` pipeline: batch size 16, prefetch, shuffle (16 to fit 4GB VRAM)
  6. Visualize augmented samples to verify correctness

### Phase 3: Baseline CNN
- **Focus:** Establish performance floor with simple model
- **Deliverables:** Baseline CNN, training curves (partial)
- **Execution mode:** notebook
- **DSM references:** DSM 1.0 (Notebook Collaboration Protocol)
- **Success criteria:** Baseline model trained and evaluated; accuracy recorded as reference point
- **Activities:**
  1. Build simple CNN: 3-4 Conv2D+MaxPool blocks → Flatten → Dense → 102 softmax
  2. Compile with `sparse_categorical_crossentropy`, Adam (lr=1e-3)
  3. Train for 20-30 epochs with early stopping (patience=5)
  4. Evaluate on test set: accuracy, macro F1
  5. Plot training/validation curves
  6. Record baseline accuracy (expected: 40-60%)

### Phase 4: Transfer Learning (3-phase progressive fine-tuning)
- **Focus:** Train EfficientNetB0 locally with progressive unfreezing
- **Deliverables:** Transfer learning model, evaluation report
- **Execution mode:** notebook
- **DSM references:** DSM 1.0 (Notebook Collaboration Protocol)
- **Success criteria:** Test accuracy ≥ 90%; confusion matrix and classification report generated
- **VRAM budget:** EfficientNetB0 at 224×224, batch 16 ≈ 2-3GB. Fits Quadro T1000 (4GB).
- **Activities:**
  1. Load EfficientNetB0 with ImageNet weights, `include_top=False`, input shape (224, 224, 3)
  2. **Phase 4a -- Head training:**
     - Freeze entire backbone
     - Add: GlobalAveragePooling2D → Dropout(0.4) → Dense(102, softmax)
     - Compile: Adam lr=1e-3, `sparse_categorical_crossentropy`, label_smoothing=0.1
     - Train 10 epochs
  3. **Phase 4b -- Partial unfreeze:**
     - Unfreeze top ~30% of backbone layers
     - Compile: Adam lr=1e-4, cosine decay schedule
     - Train 15 epochs with early stopping (patience=7)
  4. **Phase 4c -- Full fine-tune (if needed):**
     - Unfreeze all layers
     - Compile: Adam lr=1e-5
     - Train 10 epochs with early stopping (patience=5)
  5. Evaluate on test set: accuracy, precision, recall, F1 (macro), confusion matrix
  6. Compare against baseline
  7. Identify top-10 most confused class pairs from confusion matrix
  8. If accuracy < 90%, consider B3 on Colab (COULD deliverable)

---

## Phase Boundary Checklist (intra-sprint)
- [ ] Update methodology.md with phase observations and scores
- [ ] Create checkpoint if significant milestone reached
- [ ] Log decisions made during phase (dsm-docs/decisions/)
- [ ] Update blog materials if insights worth sharing

---

## Open Design Questions
1. Does TFDS `oxford_flowers102` expose train/val/test or only train/test in the current version?
2. Should we save the trained model (SavedModel format)? (Resolved: not pursued, project finalized after Sprint 1.)
3. Class weights vs. oversampling -- which to try first given the merged training set?

---

## How to Resume
1. Read this sprint plan
2. Read the most recent checkpoint in dsm-docs/checkpoints/
3. Open the notebook locally, check which phase was last completed
4. Review any decision logs in dsm-docs/decisions/

---

## Sprint Boundary Checklist
- [ ] Checkpoint document created (dsm-docs/checkpoints/)
- [ ] Feedback files updated (backlogs, methodology)
- [ ] Decision log updated with sprint decisions
- [ ] Blog journal entry written
- [ ] Repository README updated (status, results, structure)

---

## Key Technical References

| Topic | Finding | Source |
|-------|---------|--------|
| Dataset splits | Train: 1,020 (10/class), Val: 1,020 (10/class), Test: 6,149 (40-258/class) | Nilsback & Zisserman 2008 |
| Primary backbone | EfficientNetB0 at 224×224 (fits 4GB VRAM locally) | Hardware constraint |
| Stretch backbone | EfficientNetB3 at 300×300 (Colab fallback if OOM) | Research phase |
| Resolution | 224×224 for B0; 300×300 for B3 stretch | Research phase |
| Preprocessing | EfficientNet expects [0,255] not [0,1]; use `preprocess_input` | Research phase |
| Label smoothing | 0.1 prevents overconfident predictions on similar species | Research phase |
| Augmentation | Horizontal flip, rotation, zoom, color jitter; NO vertical flip | Research phase |
| Batch size | 16 (constrained by 4GB VRAM; also appropriate for small training set) | Hardware + research |
| Local GPU | Quadro T1000 Max-Q, 4GB VRAM, CUDA 12.8 | Local machine |
| Realistic target | 95-97% with good fine-tuning; 90%+ is solid portfolio result | Papers With Code |
