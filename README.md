# EPITFlow-X

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch 2.0+](https://img.shields.io/badge/PyTorch-2.0+-red.svg)](https://pytorch.org/)

Official implementation of **"EPITFlow-X: Breaking Rank Collapse in Antibody-Antigen Affinity Prediction via Contrastive Ranking Sharpening"** (Bioinformatics Advances, under review).

## Key Results

| Metric | Bio-OS Baseline | EPITFlow-X (Full) |
|--------|----------------|-------------------|
| Spearman ρ ↑ | 0.15 | **0.7666** |
| Pearson r ↑ | 0.35 | **0.8218** |
| MSE ↓ | 1.25 | **0.0318** |
| NDCG@10 ↑ | 0.42 | **0.62** |
| PredUnique ↑ | 1,500 | **5,503** |

## Architecture Overview

EPITFlow-X is a cross-attention Transformer framework that solves **rank collapse** in antibody-antigen affinity prediction through:

- **Contrastive Ranking Sharpening (CRS) Loss**: Explicitly penalizes overly similar predictions within each batch, forcing discriminative ranking without increasing parameters.
- **Robust Data Curation**: Handles extreme experimental outliers (up to $10^{19}$) via Huber loss and outlier removal.
- **Linear Temperature Scaling**: Preserves full dynamic range during ensemble inference, unlike Sigmoid-based compression.

## Quick Start

### Installation

```bash
git clone https://github.com/[yourname]/EPITFlow-X.git
cd EPITFlow-X
pip install -r requirements.txt
```

### Training

```bash
python train.py \
  --data_dir /path/to/bioos_data \
  --embed_dim 256 \
  --num_layers 4 \
  --num_heads 8 \
  --batch_size 128 \
  --epochs 15 \
  --lr 5e-5 \
  --crs_margin 0.05 \
  --crs_warmup_epoch 5
```

### Inference

```bash
python predict.py \
  --checkpoint best_model_spearman_7666.pt \
  --input test_sequences.csv \
  --output predictions.csv \
  --ensemble 5 \
  --temperature 0.8
```

## Project Structure

```
EPITFlow-X/
├── checkpoints/          # Saved model checkpoints (Git LFS)
├── data/                 # Data curation scripts
│   ├── clean_data.py
│   └── outlier_filter.py
├── models/               # Model architecture
│   ├── epitflow_x.py
│   ├── crs_loss.py
│   └── huber_loss.py
├── scripts/              # Training and evaluation scripts
│   ├── train.sh
│   └── reproduce.sh
├── utils/                # Helper functions
│   ├── metrics.py
│   └── temperature_scaling.py
├── paper/                # Manuscript source
│   ├── main.tex
│   └── fig1_training_curves.pdf
├── train.py              # Main training script
├── predict.py            # Inference script
├── requirements.txt
├── LICENSE
└── README.md
```

## Citation

If you use EPITFlow-X in your research, please cite:

```bibtex
@article{epitflowx2026,
  title={EPITFlow-X: Breaking Rank Collapse in Antibody-Antigen Affinity Prediction via Contrastive Ranking Sharpening},
  author={Anonymous},
  journal={Bioinformatics Advances},
  year={2026},
  note={Under review}
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
