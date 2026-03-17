# Liver-tumor-segmentation-using-2D-Unet
# 🧠 Liver Tumor Segmentation using 2D U-Net

## 📌 Project Overview

This project focuses on automatic liver tumor segmentation from CT scan images using a **2D U-Net architecture**. The goal is to accurately identify and segment liver tumors to assist in medical diagnosis and treatment planning.

---

## 📂 Dataset

We use the **Medical Segmentation Decathlon (MSD) - Liver Dataset**.

* **Modality:** CT scans
* **Task:** Liver and tumor segmentation
* **Data Type:** 3D volumetric scans (converted to 2D slices)
* **Annotations:** Pixel-wise segmentation masks

### Classes:

* 0 → Background
* 1 → Liver
* 2 → Tumor

---

## ⚙️ Preprocessing Pipeline

Since the dataset is 3D and the model is 2D, preprocessing is essential.

### Steps:

1. **3D to 2D Conversion**

   * Extract axial slices from volumetric CT scans

2. **Resizing**

   * Standardized to fixed resolution (e.g., 256×256)

3. **Intensity Normalization**

   * Hounsfield Units clipped (e.g., [-100, 400])
   * Min-max normalization applied

4. **Foreground Sampling**

   * Increased sampling of slices containing tumors
   * Helps handle class imbalance

5. **Data Augmentation**

   * Horizontal & vertical flips
   * Rotation
   * Brightness/contrast adjustment
   * Gaussian noise

6. **Caching**

   * Preprocessed slices saved as `.npy` files for faster training

---

## 🧠 Model Architecture

* 2D U-Net (Encoder–Decoder architecture)
* Skip connections for spatial information preservation
* Convolution + ReLU blocks
* Max pooling for downsampling
* Transposed convolution for upsampling

---

## 🏋️ Training

### Configuration:

* **Loss Function:** Dice Loss (primary)
* **Optimizer:** Adam
* **Learning Rate:** ~1e-4
* **Batch Size:** Depends on GPU (typically 4–16)
* **Mixed Precision:** Enabled (FP16)
* **Gradient Accumulation:** Used for larger effective batch size

### Strategy:

* Train on cached `.npy` slices
* Validate using Dice score
* Save best model based on validation performance

---

## 📊 Evaluation Metrics

### Dice Coefficient (Primary Metric)

The model is evaluated using the **Dice Score**, which measures overlap between predicted and ground truth masks.

[
Dice = \frac{2TP}{2TP + FP + FN}
]

* Well-suited for medical segmentation
* Handles class imbalance effectively
* Used for both training and validation

---

## ⚠️ Future Evaluation Improvements

Additional metrics such as the following are important for a more comprehensive evaluation and will be included in future versions:

* **IoU (Jaccard Index)** → stricter overlap measurement
* **Precision & Recall** → false positive/negative analysis
* **Hausdorff Distance (HD / HD95)** → boundary accuracy

---

## 🧪 Testing & Inference

* Load trained model weights
* Perform slice-wise prediction
* Reconstruct 3D volumes from 2D outputs
* Apply thresholding to obtain segmentation masks

### Outputs:

* Predicted masks
* Overlay visualizations

---

## 📁 Project Structure

```
├── 01_preprocessing_msd_to_npy_v2.ipynb
├── 02_train_unet_from_cache_v2.ipynb
├── 03_test_inference.ipynb
├── models/
├── outputs/
├── data/
└── README.md
```

---

## 💻 Hardware Requirements

* GPU recommended (T4 or better)
* RAM ≥ 8GB
* Sufficient storage for dataset

---

## ⚠️ Challenges

* Severe class imbalance (small tumor regions)
* Memory constraints for volumetric data
* Difficulty in detecting small tumors

---

## 📈 Future Work

* Add advanced evaluation metrics (IoU, Precision, Recall, HD95)
* Extend to 3D U-Net
* Incorporate attention mechanisms
* Experiment with transformer-based segmentation models

---

## 🙌 Acknowledgements

* Medical Segmentation Decathlon (MSD)
* U-Net (Ronneberger et al.)

---
