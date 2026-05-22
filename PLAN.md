# Project Plan — dental-disease-detection

**Student:** Shree Ramji | ec25075@qmul.ac.uk  
**Supervisor:** Dr. Dimitrios  
**Institution:** Queen Mary University of London  
**Programme:** MSc Electronic Engineering and Computer Science  

---

## Decisions Made

| Decision | Choice | Reason |
|---|---|---|
| Dataset | DENTEX (MICCAI 2023) | Only dataset with both classification + localisation annotations, published baselines, CC-BY licence |
| Modality | Panoramic Dental X-rays | Most datasets available, best annotated |
| Task | Classification + Localisation | Matches project definition objectives |
| Baseline | HierarchicalDet (AP50: 0.550) | Official DENTEX challenge organiser baseline |
| Classification Models | ResNet50, DenseNet121, ViT-B/16 | CNN vs CNN vs Transformer comparison |
| Detection Model | YOLOv8 (primary), Faster R-CNN (if time) | YOLOv8 easier to set up, YOLO format supported by DENTEX |
| Explainability | Grad-CAM | Applied to ResNet50 and DenseNet121 |
| Model Review | CNN / ViT / Mamba / VLMs | Comparative written review in dissertation — no Mamba or VLM implementation |
| Demo App | Django (mobile-responsive via Bootstrap) | Proof-of-concept system artefact |
| CT / CBCT | Literature review only — no implementation | Out of scope for implementation, documented in dataset table |
| Mobile App | Not in scope | Too complex alongside ML work — Django is mobile-responsive instead |
| MLOps Tool | Weights & Biases (W&B) | Experiment tracking — logs hyperparameters, metrics, and model weights automatically |

---

## Dataset Documentation

| Dataset | Modality | Annotation | Diseases | Images | Role |
|---|---|---|---|---|---|
| DENTEX (MICCAI 2023) | Panoramic X-ray | Bounding boxes | Caries, Periapical lesions, Impacted teeth, Deep caries | 1,005 labelled + 1,571 unlabelled | Primary — classification + localisation |
| Tufts Dental Database | Panoramic X-ray | Classification labels | Multiple abnormalities (5-level schema) | 1,000 | Secondary — classification + Grad-CAM |
| Periapical Disease Dataset (2024) | Periapical X-ray | Segmentation masks | Periapical lesions, Caries, Pulpal disease | 929 | Supplementary — localisation |
| Dental OPG X-Ray Dataset (2024) | Panoramic X-ray | Bounding boxes | Caries, Fractures, Infections, Impacted, Healthy | 232 | Supplementary — prototyping |
| Roboflow Dental Collections | Panoramic X-ray | Bounding boxes (YOLO) | Caries, Bone loss, Periapical lesions, Fractures | Varies | Supplementary — augmentation |
| CT datasets (TBD) | CT scan | Varies | Bone loss, Cysts, Root anatomy | TBD | Literature review only |
| CBCT datasets (TBD) | CBCT | Varies | Varies | TBD | Literature review only |

---

## Model Review (Dissertation Chapter — No Implementation)

These models will be reviewed and compared in the dissertation literature review chapter. Implementation is only considered where feasible within project scope.

| Model Type | Examples | Suitability for dental imaging | Implementation |
|---|---|---|---|
| CNN-based | ResNet50, DenseNet121 | Strong — proven in medical imaging | ✅ Implemented |
| Transformer-based | ViT-B/16 | Strong — global attention useful for X-rays | ✅ Implemented |
| State-space models | Mamba, VMamba | Promising but poorly documented | ❌ Review only |
| Multimodal LLMs | LLaVA, BioViL, GPT-4V | Needs 40–80GB VRAM, expert fine-tuning | ❌ Review only |
| VLMs / MLLMs | BioViL-T, MedFlamingo | Research stage for dental imaging | ❌ Review only |

---

## Phase-by-Phase Plan

### Phase 1 — Data Collection & EDA
**Timeline:** Now → March  
**Notebook:** `01_eda.ipynb`

- [ ] Download DENTEX dataset from Hugging Face
- [ ] Understand DENTEX annotation format (COCO JSON — quadrant, enumeration, diagnosis)
- [ ] Visualise sample images per disease class
- [ ] Plot class distributions — number of samples per disease
- [ ] Analyse image dimensions, quality, and variation across institutions
- [ ] Document dataset statistics table (diseases, modality, sample counts) for dissertation
- [ ] Review and document CT/CBCT datasets for literature review section
- [ ] Complete literature review — read CNN / ViT / Mamba / VLM papers
- [ ] Review HierarchicalDet paper (arXiv:2303.06500) — understand the baseline

---

### Phase 2 — Preprocessing
**Timeline:** March – April  
**Notebook:** `02_preprocessing.ipynb`

- [ ] Resize all images to consistent dimensions (640x640 for YOLO, 224x224 for CNN)
- [ ] Normalise pixel values using ImageNet mean and std
- [ ] Apply augmentation: horizontal flip, brightness, contrast, rotation, zoom
- [ ] Stratified train / val / test split — 70% / 15% / 15% by disease class
- [ ] Convert COCO JSON annotations → YOLO format for YOLOv8 training
- [ ] Save processed dataset to Google Drive immediately
- [ ] Write `src/dataset.py` — reusable PyTorch Dataset class (if time)

---

### Phase 3 — Classification Models
**Timeline:** April – May  
**Notebook:** `03_classification.ipynb`

**Model 1 — ResNet50 (CNN baseline)**
- [ ] Load ResNet50 with ImageNet pre-trained weights
- [ ] Replace final classification head for DENTEX disease classes
- [ ] Fine-tune on DENTEX training set
- [ ] Save best weights: `models/resnet50_best.pt`

**Model 2 — DenseNet121 (CNN improved)**
- [ ] Load DenseNet121 with ImageNet pre-trained weights
- [ ] Replace final classification head
- [ ] Fine-tune on DENTEX training set
- [ ] Save best weights: `models/densenet121_best.pt`

**Model 3 — ViT-B/16 (Transformer)**
- [ ] Load ViT-B/16 from HuggingFace with pre-trained weights
- [ ] Fine-tune on DENTEX training set
- [ ] Save best weights: `models/vit_best.pt`
- [ ] Compare against ResNet50 and DenseNet121 — CNN vs Transformer analysis

**Evaluation (all three models)**
- [ ] Plot training/validation loss and accuracy curves
- [ ] Evaluate: Accuracy, Precision, Recall, F1-score, ROC-AUC
- [ ] Plot confusion matrices
- [ ] Three-way comparison table in dissertation
- [ ] Write `src/models.py`, `src/train.py`, `src/evaluate.py` (if time)

**W&B Experiment Tracking**
- [ ] Set up W&B project — `dental-disease-detection`
- [ ] Log all hyperparameters, metrics, and weights for every run
- [ ] Export comparison table from W&B for dissertation results chapter

**GPU:** University 15GB GPU (all three models fit comfortably)

---

### Phase 4 — Detection & Localisation
**Timeline:** May – June  
**Notebook:** `04_detection.ipynb`

**Model 1 — YOLOv8 (primary)**
- [ ] Set up DENTEX in YOLO format (already done in preprocessing)
- [ ] Train YOLOv8 on disease localisation task
- [ ] Visualise predicted bounding boxes on test images
- [ ] Save best weights: `models/yolov8_best.pt`

**Model 2 — Faster R-CNN (if time)**
- [ ] Set up DENTEX in COCO format
- [ ] Train Faster R-CNN
- [ ] Save best weights: `models/fasterrcnn_best.pt`

**Evaluation**
- [ ] Evaluate: AP50, AP75, mAP, AR per disease class
- [ ] Compare directly against HierarchicalDet baseline (AP50: 0.550)
- [ ] Focus comparison on Diagnosis and Quadrant subtasks (not Enumeration)
- [ ] Log all YOLOv8 runs to W&B for experiment tracking

**GPU:** University 15GB GPU or Lightning AI A100

---

### Phase 5 — Explainability
**Timeline:** June  
**Notebook:** `05_gradcam.ipynb`

- [ ] Apply Grad-CAM to ResNet50 on test X-ray images
- [ ] Apply Grad-CAM to DenseNet121 on test X-ray images
- [ ] Apply Grad-CAM to ViT-B/16 (attention rollout method for transformers)
- [ ] Visualise heatmap overlays per disease class
- [ ] Analyse whether heatmaps align with clinically relevant regions
- [ ] Discuss limitations of Grad-CAM for medical imaging
- [ ] Write `src/gradcam.py` (if time)

---

### Phase 6 — Django Demo App
**Timeline:** July (bonus — only after dissertation is mostly written)  
**Location:** `webapp/`  
**Note:** This is Tier 3 — project is complete without it

- [ ] Set up Django project inside `webapp/`
- [ ] Create upload page — user uploads a dental X-ray image
- [ ] Load saved model weights (.pt file) inside Django view
- [ ] Run inference — return predicted disease class + confidence score
- [ ] Apply Grad-CAM and display heatmap overlaid on uploaded image
- [ ] Make mobile-responsive using Bootstrap (no native mobile app needed)
- [ ] Screenshot for dissertation system artefact section

**Stack:** Python · Django · PyTorch · Bootstrap  
**Note:** No database, no login system — just upload → predict → show result

---

### Phase 7 — Dissertation Writing
**Timeline:** July – August

- [ ] Chapter 1: Introduction — problem statement, motivation, objectives
- [ ] Chapter 2: Literature Review
  - [ ] AI in dental radiology
  - [ ] CNN-based models (ResNet, DenseNet)
  - [ ] Transformer-based models (ViT)
  - [ ] State-space models (Mamba) — review only
  - [ ] Multimodal models (VLMs, MLLMs) — review only
  - [ ] Dental imaging datasets (X-ray, CT, CBCT)
  - [ ] Explainability in medical AI (Grad-CAM)
- [ ] Chapter 3: Methodology — data, preprocessing, model choices
- [ ] Chapter 4: Implementation — code walkthrough, training setup
- [ ] Chapter 5: Results & Evaluation — all metrics, comparison to baseline
- [ ] Chapter 6: Conclusion & Future Work
- [ ] Appendix: Dataset documentation table

---

## GPU Strategy

| Environment | VRAM | Use for |
|---|---|---|
| University GPU | 15 GB | Primary training — ResNet50, DenseNet121, ViT, YOLOv8 |
| Lightning AI A100 | 40 GB | Fast large-batch experiments, YOLOv8 at high resolution |
| Google Colab Free | ~12 GB | Quick tests, debugging, small experiments |
| Kaggle | 16 GB (P100) | Backup free GPU option |

**Critical habits:**
- Mount Google Drive at the top of every Colab notebook
- Save model weights with `torch.save()` after every training run
- Name weight files with version and date: `resnet50_best_v1_2025-05-01.pt`
- Never close a Colab session without saving weights first

---

## MLOps — Weights & Biases (W&B)

**Tool:** Weights & Biases  
**Purpose:** Experiment tracking across all training runs  
**Used in:** `03_classification.ipynb` and `04_detection.ipynb` only  
**Dashboard:** https://wandb.ai  

### Setup
```bash
pip install wandb
wandb login  # paste your API key from wandb.ai/authorize
```

### How to use in every training notebook
```python
import wandb

# Start a run — give it a descriptive name
wandb.init(
    project="dental-disease-detection",
    name="ResNet50_lr0.001_batch32_epoch50",
    config={
        "model": "ResNet50",
        "learning_rate": 0.001,
        "batch_size": 32,
        "epochs": 50,
        "optimizer": "Adam",
        "loss": "CrossEntropyLoss",
        "class_weights": True,
        "image_size": 224,
        "dataset": "DENTEX"
    }
)

# Inside training loop — log metrics every epoch
wandb.log({
    "epoch": epoch,
    "train_loss": train_loss,
    "val_loss": val_loss,
    "val_accuracy": val_acc,
    "val_f1": val_f1
})

# At the end — save model weights to W&B
wandb.save("models/resnet50_best.pt")
wandb.finish()
```

### What W&B logs automatically
- Loss and accuracy curves per epoch
- Hyperparameters for every run
- Model weights (stored in cloud)
- GPU utilisation and memory
- Training time per epoch
- Side-by-side comparison of all runs

### Why this matters for dissertation
- Reproducibility — every experiment is fully documented
- Supervisor can view results via shared link — no need to screenshot
- Results chapter writes itself — export tables directly from W&B
- Shows professional MLOps practice 

### Naming convention for runs
```
{model}_{lr}_{batch}_{epochs}_{notes}
e.g. ResNet50_lr0.001_batch32_ep50_baseline
     DenseNet121_lr0.0001_batch16_ep50_weighted
     ViT_lr0.00005_batch8_ep30_finetuned
     YOLOv8_640_ep100_baseline
```

---

## Baseline Reference

**Model:** HierarchicalDet (Hamamci et al., 2023)  
**Paper:** arXiv:2303.06500  
**Challenge:** DENTEX MICCAI 2023 — ranked 13th out of all participants  
**Code:** https://github.com/ibrahimethemhamamci/HierarchicalDet

| Subtask | AP | AP50 | AP75 | AR |
|---|---|---|---|---|
| Quadrant | 0.387 | 0.603 | 0.463 | 0.672 |
| Diagnosis | 0.372 | 0.601 | 0.422 | 0.653 |
| Enumeration | 0.287 | 0.446 | 0.342 | 0.550 |
| Aggregate | 0.348 | 0.550 | 0.409 | 0.625 |

**Your realistic target:** AP50 of 0.50–0.60 on Diagnosis detection with YOLOv8  
**Focus on:** Quadrant and Diagnosis subtasks only — skip Enumeration (tooth numbering)

---

## Progress Tracker

Update checkboxes as each phase is completed and commit the change.

| Phase | Notebook / Deliverable | Status |
|---|---|---|
| 1 | 01_eda.ipynb | 🔲 Not started |
| 2 | 02_preprocessing.ipynb | 🔲 Not started |
| 3 | 03_classification.ipynb (ResNet50) | 🔲 Not started |
| 3 | 03_classification.ipynb (DenseNet121) | 🔲 Not started |
| 3 | 03_classification.ipynb (ViT-B/16) | 🔲 Not started |
| 4 | 04_detection.ipynb (YOLOv8) | 🔲 Not started |
| 4 | 04_detection.ipynb (Faster R-CNN) | 🔲 Not started |
| 5 | 05_gradcam.ipynb | 🔲 Not started |
| 6 | Django demo app | 🔲 Not started |
| 7 | Dissertation | 🔲 Not started |

---

## Key Links

| Resource | URL |
|---|---|
| DENTEX Dataset | https://huggingface.co/datasets/ibrahimhamamci/DENTEX |
| DENTEX GitHub | https://github.com/ibrahimethemhamamci/DENTEX |
| DENTEX Paper | https://arxiv.org/abs/2305.19112 |
| HierarchicalDet Paper | https://arxiv.org/abs/2303.06500 |
| HierarchicalDet Code | https://github.com/ibrahimethemhamamci/HierarchicalDet |
| DENTEX Grand Challenge | https://dentex.grand-challenge.org/ |
| Tufts Dental Database | https://tdd.ece.tufts.edu/ |
| Periapical Dataset | https://pmc.ncbi.nlm.nih.gov/articles/PMC11177072/ |
| Roboflow Dental | https://universe.roboflow.com/search?q=dental+x+ray |
| Weights & Biases | https://wandb.ai |
| W&B Quickstart | https://docs.wandb.ai/quickstart |

