# Automated Polyp Segmentation in Endoscopic Imagery

This repository contains the code and methodology for a Medical Image Analysis pipeline designed to automatically segment colorectal polyps from endoscopic imagery. The project benchmarks a state-of-the-art instance segmentation model (YOLOv8) against a custom-built, lightweight PyTorch U-Net architecture.

## 📊 Project Performance Summary
Tested on a pre-processed subset of the **HyperKvasir** dataset (80/10/10 split).

| Architecture | Precision | Recall (Sensitivity) | Dice Score |
| :--- | :---: | :---: | :---: |
| **YOLOv8-Seg** (Baseline) | **89.8%** | 87.1% | 0.842 |
| **Custom PyTorch U-Net** | 88.2% | **90.0%** | **0.888** |

*Clinical Note: The U-Net's higher Recall (90.0%) makes it the preferred architecture for this medical context, as minimizing False Negatives (missed polyp tissue) is critical for cancer screening.*

## ⚙️ Methodology & Pipeline
### 1. Pre-processing
Endoscopic images suffer from severe camera glare and specular reflection. We implemented a deterministic pre-processing pipeline:
* **Color Space Conversion:** Images converted to LAB space to isolate illumination (L).
* **Inpainting:** TELEA algorithm deployed to remove specular reflections without chromatic hallucination.
* **Contrast Enhancement:** CLAHE applied strictly to the L-channel to enhance mucosal vascularity.

### 2. Custom U-Net Architecture
Built entirely from scratch in PyTorch to ensure pixel-level spatial fidelity:
* **Lightweight Encoder:** 4-tier downsampling (32 -> 64 -> 128 -> 256) to prevent overfitting on limited medical data.
* **Hybrid Optimization:** Utilized a combined **BCE-Dice Loss** function. BCE prevented zero-gradient starvation during early epochs, while Dice Loss tightly wrapped the predicted masks to the ground truth.
* **Dynamic Tuning:** `ReduceLROnPlateau` scheduler implemented to scale down learning rates upon loss stagnation, eliminating boundary "spillage".

## 🚀 How to Run
This project was built and executed in Google Colab.
1. Clone this repository or open the `.ipynb` file directly in Google Colab.
2. The notebook expects the dataset to be mounted via Google Drive. Update the `image_folder` and `mask_folder` paths in the Dataset initialization block to match your local or cloud directories.

## 💾 Pre-trained Weights
You can download the trained `.pth` and `.pt` files to test the inference engine immediately:
* [Link to YOLOv8 Weights] *(Insert your Google Drive share link here)*
* [Link to Custom U-Net Weights] *(Insert your Google Drive share link here)*

## 👥 Authors
* **R Sarang**
* *Indian Institute of Information Technology, Design and Manufacturing (IIITDM), Kancheepuram*
