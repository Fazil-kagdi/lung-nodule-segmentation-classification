# Lung-Nodule segmentation & classification
Deep learning pipeline for lung nodule detection and classification using LIDC-IDRI dataset. Includes preprocessing, UNet-based segmentation, ResNet-based malignancy classification, and full training/visualization workflows.

## 🧠 Model Overview

- **Segmentation**: 2D U-Net trained on CT slices with Dice + BCE loss.
- **Classification**: ResNet18 fine-tuned on extracted nodule patches.

## 📦 Requirements

To run this project, make sure you have the following packages installed:
```bash
pip install -r requirements.txt
```

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

## 📓 Step 1: Running the Pipeline

After installing requirements and running the preprocessing script, open the main Jupyter notebook to run the complete pipeline:

The notebook includes:

#### 🔹 Segmentation Model Training (U-Net)
- Trains on 2D slices from preprocessed CT scans.
- Uses **Dice + BCE** loss with a positive pixel weighting strategy.
- Saves the best model as `best_model.pth`.

#### 🔹 Patch Extraction
- Extracts **64×64 patches** around nodules from the images using bounding boxes.
- Patches are saved under:
  - `patches/benign/`
  - `patches/malignant/`

#### 🔹 Classification Model Training (ResNet18)
- Classifies each patch as **benign** or **malignant**.
- Best model saved as `best_classifier.pth`.

#### 🔹 Joint Visualization
- Randomly picks a **nodule slice and patch**.
- Displays:
  - Original CT image  
  - Ground truth segmentation  
  - U-Net prediction  
  - Classification result vs. ground truth

## 🏋️ Model Weights
 - You can download the weights for the segmentation model [here](https://drive.google.com/file/d/196mID1SP9VXOjTiSONt3Oh_Ek0Xmg-in/view?usp=sharing)
  
 - You can download the weights for the classifier [here](https://drive.google.com/file/d/1hYrXjUZSZKT-t5ucD89ESvbKHI3_kPzN/view?usp=sharing)

## 📊 Results

- **Segmentation Dice Score**: ~0.75 (on validation set of 30 patients)
- **Classification Accuracy**: ~85% (benign vs malignant)
  ![Demo Screenshot](output.png)

## 🗂️ Dataset

- Source: [LIDC-IDRI Dataset](https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI)
- We used a **subset of 150 patients** for training and validation.
- Preprocessing handled using [`pylidc`](https://github.com/jaeho3690/LIDC-IDRI-Preprocessing).

## 📚 References

- MONAI: Medical Open Network for AI - https://monai.io/
- Jaeho Lee. [LIDC-IDRI-Preprocessing](https://github.com/jaeho3690/LIDC-IDRI-Preprocessing). GitHub repository.
- Matthew Hancock. [pylidc](https://github.com/notmatthancock/pylidc). GitHub repository.
- The Cancer Imaging Archive. [LIDC-IDRI](https://nbia.cancerimagingarchive.net/nbia-search/?CollectionCriteria=LIDC-IDRI). National Biomedical Imaging Archive.


