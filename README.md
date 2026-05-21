# Dental-disease-detection

Deep learning-based detection and localisation of oral diseases from panoramic dental X-rays using CNN, YOLOv8, and Grad-CAM explainability | MSc Project — Queen Mary University of London

---

## Project Overview

This project develops a deep learning system for automated detection and localisation of oral diseases from panoramic dental X-rays. It uses CNN-based models (ResNet50, DenseNet121) for disease classification, YOLOv8 for disease localisation, and Grad-CAM for explainability.

**Student:** Shree Ramji  
**Email:** ec25075@qmul.ac.uk  
**Supervisor:** Dr. Dimitrios  
**Institution:** Queen Mary University of London  
**Programme:** MSc Electronic Engineering and Computer Science  

---

## 📋 Project Plan
See [PLAN.md](PLAN.md) for the full phase-by-phase project plan, dataset decisions, model choices, and progress tracker.

---

## Dataset

**Primary Dataset:** DENTEX (MICCAI 2023)  
- 1,005 fully annotated panoramic dental X-rays  
- 1,571 unlabelled X-rays (optional pre-training)  
- Diseases: Caries, Periapical Lesions, Impacted Teeth, Deep Caries  
- Source: [Hugging Face](https://huggingface.co/datasets/ibrahimhamamci/DENTEX)  
- Licence: CC-BY  

**Baseline to beat:** HierarchicalDet (DENTEX official baseline) — Aggregate AP50: 0.550

---

## Project Structure

```
dental-disease-detection/
│
├── notebooks/
│   ├── 01_eda.ipynb              # Dataset exploration & visualisation
│   ├── 02_preprocessing.ipynb   # Preprocessing & augmentation
│   ├── 03_classification.ipynb  # ResNet50 & DenseNet121 training
│   ├── 04_detection.ipynb       # YOLOv8 localisation training
│   └── 05_gradcam.ipynb         # Grad-CAM explainability
│
├── src/
│   ├── dataset.py               # Dataset loading & augmentation
│   ├── models.py                # Model definitions
│   ├── train.py                 # Training loops
│   ├── evaluate.py              # Evaluation metrics
│   └── gradcam.py               # Grad-CAM implementation
│
├── webapp/                      # Django demo application
│   └── core/
│       └── templates/
│
├── models/                      # Saved model weights (not tracked by Git)
├── data/                        # Dataset files (not tracked by Git)
├── requirements.txt             # Python dependencies
└── README.md
```

---

## Installation

```bash
# Clone the repository
git clone https://github.com/anshs990/dental-disease-detection
cd dental-disease-detection

# Install dependencies
pip install -r requirements.txt
```

---

## How to Run

Run notebooks in order:

```
1. notebooks/01_eda.ipynb
2. notebooks/02_preprocessing.ipynb
3. notebooks/03_classification.ipynb
4. notebooks/04_detection.ipynb
5. notebooks/05_gradcam.ipynb
```

Each notebook is self-contained with markdown explanations throughout.

---

## Models

| Model | Task | Metrics |
|---|---|---|
| ResNet50 | Classification | Accuracy, F1, ROC-AUC |
| DenseNet121 | Classification | Accuracy, F1, ROC-AUC |
| YOLOv8 | Localisation | AP50, AP75, mAP, AR |

---

## Results

*To be updated as experiments are completed.*

| Model | Accuracy | F1 | ROC-AUC |
|---|---|---|---|
| ResNet50 | - | - | - |
| DenseNet121 | - | - | - |

| Model | AP50 | AP75 | mAP | AR |
|---|---|---|---|---|
| HierarchicalDet (baseline) | 0.550 | 0.409 | 0.348 | 0.625 |
| YOLOv8 (ours) | - | - | - | - |

---

## Requirements

- Python 3.10+
- PyTorch 2.0+
- CUDA-compatible GPU (minimum 8GB VRAM recommended)
- See `requirements.txt` for full list

---

## Licence

MIT
