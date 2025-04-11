# Lung-Nodule segmentation & classification
Deep learning pipeline for lung nodule detection and classification using LIDC-IDRI dataset. Includes preprocessing, UNet-based segmentation, ResNet-based malignancy classification, and full training/visualization workflows.

## 🧠 Model Overview

- **Segmentation**: 2D U-Net trained on CT slices with Dice + BCE loss.
- **Classification**: ResNet18 fine-tuned on extracted nodule patches.

## 🧩 Step 0: Preprocess LIDC-IDRI to Extract 2D Slices

Before training or running any models, you **must extract 2D image slices and segmentation masks** from the original LIDC-IDRI dataset.

> ⚠️ **Important:** Follow the instructions in  
> `LIDC-IDRI-Preprocessing-master/PREPROCESSING_README.md`

This step uses the [`pylidc`](https://github.com/pylidc/pylidc) library to:

- Parse raw DICOM scans
- Segment lungs
- Extract nodule masks based on consensus
- Save 2D `.npy` slices and their masks

Once completed, the following directories will be created:

LIDC-IDRI-Preprocessing-master/
├── data/
│   ├── Image/       # 2D CT slices as .npy
│   ├── Mask/        # Corresponding binary nodule masks
│   └── Meta/        # CSV with metadata for each slice

or just replace the entire data folder in LIDC-IDRI-Preprocessing-master with this folder you can download [here](https://drive.google.com/drive/folders/196VKBCb8DlEdzSwZHE8Mz7gHAsahyJGA?usp=sharing)

## 📊 Results

- **Segmentation Dice Score**: ~0.75 (on validation set of 30 patients)
- **Classification Accuracy**: ~85% (benign vs malignant)

## 🗂️ Dataset

- Source: [LIDC-IDRI Dataset](https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI)
- We used a **subset of 150 patients** for training and validation.
- Preprocessing handled using [`pylidc`](https://github.com/jaeho3690/LIDC-IDRI-Preprocessing).

## ⚙️ Requirements

- Python ≥ 3.8
- PyTorch
- MONAI
- torchvision
- pylidc
- numpy, pandas, matplotlib, scikit-image
