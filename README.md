# EfficientNet Flower Classification (Transfer Learning)

102-class flower species classification using EfficientNetB0 transfer learning on the Oxford Flowers 102 dataset.

**Status:** Complete (archived 2026-04-07). Sprint 1 closed at 91.90% test accuracy with TTA. No further sprints planned.

## Author

**Alberto Diaz Durana**
[GitHub](https://github.com/albertodiazdurana) | [LinkedIn](https://www.linkedin.com/in/albertodiazdurana/)

## Results

| Model | Test Accuracy | Macro F1 | Parameters | Training Time |
|-------|--------------|----------|------------|---------------|
| Baseline CNN | 24.56% | 0.1984 | 153K | 5.6 min |
| EfficientNetB0 (transfer) | 91.01% | 0.9072 | 4.18M | 8.7 min |
| + Test-Time Augmentation | **91.90%** | **0.9175** | 4.18M | +1.1 min inference |

- 3-phase progressive fine-tuning: head-only, top 30% unfreeze, full fine-tune
- Trained on 1,734 images (~10 per class), evaluated on 6,149 test images
- TTA: 5 augmented views (original, h-flip, ±90° rotation, center crop), averaged softmax

![Baseline vs Transfer Learning](notebooks/outputs/figures/evaluation/s01_d01_baseline_vs_transfer.png)

## Error analysis

The model's worst errors are between flowers that share color and petal shape.

![Most-Confused Pairs](notebooks/outputs/figures/evaluation/s01_d01_confused_pairs_visual.png)

## Run the notebook

**Option A, Google Colab (recommended):**
Upload `notebooks/flower-classification.ipynb` to Colab, select T4 GPU runtime, and run all cells.

**Option B, local:**
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements_base.txt
pip install -r requirements_project.txt
jupyter lab notebooks/flower-classification.ipynb
```

Requires a CUDA-compatible GPU (tested on Quadro T1000, 4GB VRAM).

## Folder Structure

```
efficientnet-flower-classification-transfer-learning/
├── notebooks/                  # Jupyter notebooks (main deliverables)
│   └── outputs/
│       ├── figures/            # Saved plots from notebook cells
│       └── *.weights.h5        # Cached model weights (gitignored)
├── dsm-docs/                   # DSM methodology artifacts
│   ├── plans/                  # Sprint plans
│   ├── research/               # Research notes
│   ├── checkpoints/            # Session checkpoints
│   ├── decisions/              # Architecture decisions
│   └── feedback-to-dsm/        # Methodology feedback
├── requirements_base.txt       # Core Python dependencies
└── requirements_project.txt    # Project-specific dependencies
```
