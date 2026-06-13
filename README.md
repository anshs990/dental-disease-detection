# Dental Disease Detection

Deep learning system for automated detection and localisation of oral diseases from panoramic dental X-rays using CNN classification, YOLOv8 localisation, and Grad-CAM explainability | MSc Project — Queen Mary University of London

---

## Project Overview

This project builds a complete pipeline for dental disease detection from panoramic X-rays using the DENTEX (MICCAI 2023) dataset. It covers classification, object detection, and explainability — evaluated against the official HierarchicalDet baseline.

**Student:** Shree Ramji | ec25075@qmul.ac.uk  
**Supervisor:** Dr. Dimitrios  
**Institution:** Queen Mary University of London  
**Programme:** MSc Machine Learning for Visual Data Analytics

---

## Dataset

**Primary Dataset:** DENTEX (MICCAI 2023)
- 1,005 fully annotated panoramic dental X-rays (678 train / 46 val / 242 test)
- Diseases: Caries, Periapical Lesion, Impacted Teeth, Deep Caries
- Source: [Hugging Face](https://huggingface.co/datasets/ibrahimhamamci/DENTEX)
- Licence: CC-BY

**Baseline:** HierarchicalDet (DENTEX official) — Aggregate AP50: 0.550

---

## Results

### Classification (Phase 3)

| Model | Parameters | Test Accuracy | Macro F1 | Notes |
|---|---|---|---|---|
| ResNet50 | 25M | 58.3% | 0.321 | CNN baseline |
| DenseNet121 | 8M | 52.5% | 0.314 | Dense skip connections |
| ViT-B/16 | 86M | 58.7% | 0.374 | Best F1 — global attention |

> Deep_Caries excluded from test evaluation — zero test annotations in DENTEX split.

### Detection / Localisation (Phase 4)

| Model | Parameters | mAP50 | vs Baseline | Notes |
|---|---|---|---|---|
| HierarchicalDet (baseline) | — | 0.550 | — | Trained on 3,603 images |
| YOLOv8n | 3M | 0.525 | -0.025 | Smallest |
| YOLOv8m | 26M | 0.549 | -0.001 | |
| **YOLOv8l** | **43M** | **0.558** | **+0.008** | **Best — beats baseline** |
| YOLOv8x | 68M | 0.534 | -0.016 | Overfits at this scale |

**YOLOv8l beats HierarchicalDet by +0.008 AP50 using only 678 training images (5.3× fewer than baseline).**

### Explainability (Phase 5)

| Model | Method | Caries | Impacted | Periapical | Overall |
|---|---|---|---|---|---|
| ResNet50 | Grad-CAM | ⚠️ | ✅ | ⚠️ | Moderate |
| DenseNet121 | Grad-CAM | ⚠️ | ✅ | ✅ | Strong |
| ViT-B/16 | Grad-CAM | ✅ | ⚠️ | ⚠️ | Moderate |
| ViT-B/16 | Attn Rollout | ❌ | ⚠️ | ⚠️ | Weak |

DenseNet121 produces the most clinically aligned Grad-CAM heatmaps despite lower accuracy — dense skip connections sharpen gradient flow to target layers.

---

## Project Structure

```
dental-disease-detection/
│
├── notebooks/
│   ├── 01_eda.ipynb              # Dataset exploration & class distribution
│   ├── 02_preprocessing.ipynb   # Resize, normalise, YOLO format conversion
│   ├── 03_classification.ipynb  # ResNet50, DenseNet121, ViT-B/16 training
│   ├── 04_detection.ipynb       # YOLOv8n/m/l/x training & evaluation
│   └── 05_gradcam.ipynb         # Grad-CAM + Attention Rollout explainability
│
├── models/                      # Saved model weights (not tracked by Git)
│   ├── resnet50_best.pt
│   ├── densenet121_best.pt
│   ├── vit_best.pt
│   └── yolov8l_best.pt
│
├── outputs/
│   └── gradcam/                 # Grad-CAM heatmap outputs
│       ├── comparison_grid.png
│       ├── perclass_caries.png
│       ├── perclass_impacted.png
│       ├── perclass_periapical_lesion.png
│       └── wrong_predictions.png
│
├── data/                        # Dataset files (not tracked by Git)
│   ├── processed/cnn/           # 224×224 images for classification
│   └── processed/yolo/          # 640×640 images + YOLO annotations
│
├── PLAN.md                      # Full phase-by-phase project plan
├── requirements.txt
└── README.md
```

---

## Experiment Tracking

W&B Dashboard: https://wandb.ai/shree-cloudarcade-personal/dental-disease-detection

All training runs, metrics, and hyperparameters logged — 12 runs total (YOLOv8n/m/l/x training + test eval + classification models).

---

## Installation

```bash
git clone https://github.com/anshs990/dental-disease-detection
cd dental-disease-detection
pip install -r requirements.txt
```

---

## How to Run

Run notebooks in order — each is self-contained:

```
1. notebooks/01_eda.ipynb              # EDA
2. notebooks/02_preprocessing.ipynb   # Preprocessing
3. notebooks/03_classification.ipynb  # Classification
4. notebooks/04_detection.ipynb       # YOLOv8 detection
5. notebooks/05_gradcam.ipynb         # Grad-CAM explainability
```

**GPU required.** Trained on university 15GB GPU. Minimum 8GB VRAM recommended.

---

## Progress

| Phase | Notebook | Status |
|---|---|---|
| 1 — EDA | 01_eda.ipynb | ✅ Complete |
| 2 — Preprocessing | 02_preprocessing.ipynb | ✅ Complete |
| 3 — Classification | 03_classification.ipynb | ✅ Complete |
| 4 — Detection | 04_detection.ipynb | ✅ Complete |
| 5 — Explainability | 05_gradcam.ipynb | ✅ Complete |
| 6 — Django Demo App | webapp/ | 🔲 Not started |
| 7 — Dissertation | — | 🔲 Not started |

---

## Requirements

- Python 3.10+
- PyTorch 2.0+
- ultralytics (YOLOv8)
- timm (ViT-B/16)
- grad-cam (pytorch-grad-cam)
- CUDA-compatible GPU (8GB+ VRAM)

See `requirements.txt` for full list.

---

## Licence

MIT
