# BACKLOG-001: Automating Flower Classification Using Deep Learning

**Status:** Planning
**Priority:** High
**Date Created:** 2026-04-03
**Origin:** Class portfolio project (Information Technology case)
**Author:** Berto

---

## Business Problem

A startup wants an automated flower classification system that identifies 102
species from images. Use cases: mobile plant ID apps, e-commerce cataloging,
personalized recommendations.

## Dataset

- **Source:** Oxford Flowers 102 via `tensorflow_datasets` (`oxford_flowers102`)
- **Classes:** 102 flower species
- **Images per class:** 40–258 (imbalanced)
- **Splits:** train / test (provided by TFDS)
- **Image properties:** varying sizes, lighting, backgrounds

## Success Criteria

| Metric | Target |
|--------|--------|
| Classification accuracy | ≥ 85% on test set |
| Per-class precision/recall | Balanced across classes, no class < 60% |
| Generalization | Robust to lighting/background variation |

## Preliminary Plan

### Phase 1 — EDA & Data Understanding

1. Load dataset via TFDS, inspect splits (train/test sizes)
2. Visualize class distribution, identify imbalance severity
3. Display sample images per class, note visual similarities
4. Analyze image size distribution before resizing
5. Document findings in notebook

### Phase 2 — Baseline Model

1. Preprocessing pipeline: resize 224×224, normalize [0,1]
2. Create validation split from training data (~80/20)
3. Build a simple CNN baseline (few conv layers) to establish floor performance
4. Train with `sparse_categorical_crossentropy`, Adam optimizer
5. Evaluate accuracy, confusion matrix, per-class metrics
6. Document baseline results

### Phase 3 — Transfer Learning

1. Select pre-trained backbone (EfficientNetB0 or ResNet50, ImageNet weights)
2. Freeze base layers, add custom classification head (GlobalAveragePooling → Dense → 102 softmax)
3. Train classification head only (few epochs)
4. Unfreeze top layers of backbone, fine-tune with low learning rate
5. Compare against baseline

### Phase 4 — Data Augmentation & Regularization

1. Apply augmentation: random rotation, flip, zoom, brightness/contrast
2. Add dropout / L2 regularization if overfitting observed
3. Experiment with class weights or oversampling for imbalanced classes
4. Retrain and evaluate improvement

### Phase 5 — Hyperparameter Tuning & Optimization

1. Tune: learning rate schedule, batch size, augmentation intensity
2. Try alternative backbone (e.g., EfficientNetB3) if accuracy plateau
3. Evaluate with precision, recall, F1 per class
4. Select best model configuration

### Phase 6 — Final Evaluation & Business Interpretation

1. Run best model on held-out test set
2. Generate confusion matrix, classification report
3. Identify most confused class pairs, explain visual similarity
4. Write business recommendations (deployment considerations, mobile optimization)
5. Document findings for portfolio presentation

## Technical Stack

- **Framework:** TensorFlow / Keras
- **Data:** tensorflow_datasets (oxford_flowers102)
- **Environment:** Google Colab (GPU runtime)
- **Key libraries:** tf.keras, matplotlib, numpy, sklearn (metrics)

## Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| Class imbalance hurts rare-class recall | Class weights, augmentation, oversampling |
| Overfitting on small training set | Transfer learning, augmentation, dropout |
| Visual similarity between species | Fine-grained feature extraction via deeper backbone |
| Colab GPU memory limits | Use EfficientNet (smaller footprint) over larger models |

## Open Questions

- Should we create a validation split manually or use a predefined one?
- Target deployment format (SavedModel, TFLite for mobile)?
- Any specific flower species the startup prioritizes?
