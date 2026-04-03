# Advanced DS and AI Portfolio Projects

Deep learning and AI portfolio projects, built with a structured methodology (DSM 0.2).

## Author

**Alberto Diaz Durana**
[GitHub](https://github.com/albertodiazdurana) | [LinkedIn](https://www.linkedin.com/in/albertodiazdurana/)

## Current Project

**Flower Classification (Oxford Flowers 102)** — Transfer learning with EfficientNet for 102-class flower species identification. Sprint plan: `dsm-docs/plans/sprint-1-plan.md`.

## Folder Structure

```
Advanced-DS-and-AI-Portfolio-Projects/
├── notebooks/                  # Jupyter notebooks (main deliverables)
│   └── outputs/figures/        # Saved plots from notebook cells
├── dsm-docs/                   # DSM methodology artifacts
│   ├── plans/                  # Sprint plans
│   ├── research/               # Research notes
│   ├── checkpoints/            # Session checkpoints
│   ├── decisions/              # Architecture decisions
│   ├── feedback-to-dsm/        # Methodology feedback
│   ├── blog/                   # Learning journal
│   ├── guides/                 # How-to guides
│   └── handoffs/               # Session handoffs
├── _inbox/                     # Incoming items to process
├── _references/                # External reference material
├── requirements_base.txt       # Core Python dependencies
└── requirements_project.txt    # Project-specific dependencies
```

## Quick Start

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements_base.txt
pip install -r requirements_project.txt
jupyter lab notebooks/flower-classification.ipynb
```
