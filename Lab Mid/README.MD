# HOG vs SIFT on Blood-Cell Images

**Computer Vision Lab Mid Exam** | **Name:** Amna Javed | **Reg. No:** FA23-BAI-014

A small blood-cell classifier that runs on a normal laptop. Two classical pipelines, **HOG + SVM** and **SIFT + SVM**, are built on the BloodMNIST dataset and compared on clean, rotated and noisy test images to see which one is more reliable.

> The full code and all outputs are in [`FA23_BAI_014_AMNA_JAVED__LAB_MID_CV.ipynb`](FA23_BAI_014_AMNA_JAVED__LAB_MID_CV.ipynb). Every number in this README is copied from that notebook's outputs.

---

## Table of contents

1. [Personal settings](#1-personal-settings)
2. [Project structure](#2-project-structure)
3. [How to run](#3-how-to-run)
4. [Method](#4-method)
5. [Results](#5-results)
6. [Discussion](#6-discussion)
7. [Limitations](#7-limitations)
8. [AI usage](#8-ai-usage)

---

## 1. Personal settings

The settings come from the registration number **FA23-BAI-014**.

| Setting | Rule | My value |
|---|---|---|
| Random seed | last two digits | **14** |
| HOG cell size | last digit 4 is even | **8 x 8** |
| SIFT grid size | last digit 4 is in 0 to 4 | **4 x 4 grid (16 keypoints)** |

Fixed settings used by everyone:

| Item | Value |
|---|---|
| Dataset | BloodMNIST, size 64 (`pip install medmnist`) |
| Classes | eosinophil (1), monocyte (5), neutrophil (6), platelet (7) |
| Images per class | 100 train, 30 validation, 30 test (chosen with seed 14) |
| Image preparation | grayscale, resize to 128 x 128 |
| HOG | 9 orientations, 2 x 2 cells per block |
| Classifier | `LinearSVC`, C chosen from {0.1, 1, 10} on the validation set |
| Rotation test | 20 degrees |
| Noise test | Gaussian noise, sigma = 0.05 (image scaled to 0 to 1) |

---

## 2. Project structure

```
.
├── README.md
├── FA23_BAI_014_AMNA_JAVED__LAB_MID_CV.ipynb   # executed notebook with all outputs
└── images/                                      # figures taken from the notebook outputs
    ├── q1_examples.png
    ├── q2_hog_pictures.png
    ├── q2_hog_confusion.png
    ├── q3_sift_keypoints.png
    ├── q3_grid_keypoints.png
    ├── q3_sift_confusion.png
    └── q4_transforms.png
```

---

## 3. How to run

```bash
pip install medmnist opencv-python scikit-image scikit-learn matplotlib pandas
jupyter notebook FA23_BAI_014_AMNA_JAVED__LAB_MID_CV.ipynb
```

Run all cells from top to bottom. The first run downloads BloodMNIST (about 156 MB). Everything is seeded with 14, so the same images are sampled each time.

Libraries used: `numpy`, `opencv-python` (cv2), `scikit-image`, `scikit-learn`, `matplotlib`, `pandas`, `medmnist`.

---

## 4. Method

### Q1. Data setup
- Load BloodMNIST (64 x 64 RGB) and keep the four classes. Labels 1, 5, 6, 7 are remapped to 0, 1, 2, 3.
- Sample a fixed subset with one `RandomState(14)`: 100 / 30 / 30 images per class for train / validation / test.
- Convert to grayscale and resize to 128 x 128 (`uint8`).

### Q2. HOG + SVM
- `skimage.feature.hog` with 9 orientations, 8 x 8 pixel cells, 2 x 2 cell blocks, `L2-Hys` block normalisation.
- Feature length by hand: 128 / 8 = 16 cells per side, so 15 blocks per side, so 15 x 15 = 225 blocks. Each block holds 2 x 2 x 9 = 36 values. **225 x 36 = 8100.**
- Classifier: `StandardScaler` followed by `LinearSVC`, with C chosen on the validation set.

### Q3. SIFT + SVM
- **Normal SIFT** (`cv2.SIFT_create().detectAndCompute`) gives a different number of keypoints per image. An SVM needs equal-length vectors with the same meaning in every position, so this cannot be used directly.
- **Fix: fixed grid.** 4 x 4 keypoints are placed on the 128 x 128 image (centres at 16, 48, 80, 112 on each axis), keypoint size 32. `sift.compute` gives one 128-value descriptor per point, and these are joined into one vector.
- Feature length by hand: **16 grid points x 128 = 2048.**
- Same classifier setup as HOG.

### Q4. Robustness
- Both models are **frozen** (only `predict`, never `fit`). The test set is used only here.
- **Rotation:** 20 degrees about the image centre (`cv2.warpAffine`, black border).
- **Noise:** scale to 0 to 1, add Gaussian noise with sigma 0.05, clip, convert back to `uint8`. The noisy set is generated once so both models see identical images.
- The change is applied to the image **before** feature extraction.

---

## 5. Results

### Q1. Data

| Split | X shape | y shape | Class counts |
|---|---|---|---|
| train | (400, 128, 128) | (400,) | 100 per class |
| val | (120, 128, 128) | (120,) | 30 per class |
| test | (120, 128, 128) | (120,) | 30 per class |

![Examples per class](images/q1_examples.png)

### Q2. HOG + SVM

- HOG feature length: **8100** (matches the hand calculation).
- Train matrix (400, 8100), validation matrix (120, 8100).

![HOG pictures](images/q2_hog_pictures.png)

| C | Validation accuracy |
|---|---|
| 0.1 | 0.7083 |
| 1 | 0.7083 |
| 10 | 0.7083 |

All three values tie, so the smallest **C = 0.1** was kept.

**Validation accuracy 0.7083 | Macro-F1 0.7076**

| Class | Precision | Recall | F1 |
|---|---|---|---|
| eosinophil | 0.515 | 0.567 | 0.540 |
| monocyte | 0.656 | 0.700 | 0.677 |
| neutrophil | 0.708 | 0.567 | 0.630 |
| platelet | 0.968 | 1.000 | 0.984 |

![HOG confusion matrix](images/q2_hog_confusion.png)

### Q3. SIFT + SVM

**Normal SIFT keypoints per training image**

| Statistic | Value |
|---|---|
| Average | 72.34 |
| Minimum | 11 |
| Maximum | 187 |
| Images with zero keypoints | 0 / 400 |

The counts range from 11 to 187, so normal SIFT gives vectors of different lengths and cannot feed an SVM directly.

![Normal SIFT keypoints](images/q3_sift_keypoints.png)

**Fixed grid SIFT**

- Grid 4 x 4 = 16 points, spacing 32 px, keypoint size 32.
- SIFT feature length: **2048** (16 x 128, matches the hand calculation).
- Train matrix (400, 2048), validation matrix (120, 2048).

![Grid keypoints](images/q3_grid_keypoints.png)

| C | Validation accuracy |
|---|---|
| 0.1 | 0.7000 |
| 1 | 0.6917 |
| 10 | 0.6917 |

Best **C = 0.1**.

**Validation accuracy 0.7000 | Macro-F1 0.7012**

| Class | Precision | Recall | F1 |
|---|---|---|---|
| eosinophil | 0.452 | 0.467 | 0.459 |
| monocyte | 0.606 | 0.667 | 0.635 |
| neutrophil | 0.800 | 0.667 | 0.727 |
| platelet | 0.968 | 1.000 | 0.984 |

![SIFT confusion matrix](images/q3_sift_confusion.png)

### Q4. Robustness (test set, 120 images)

![Clean, rotated and noisy example](images/q4_transforms.png)

| Model | Clean accuracy | Rotated accuracy | Noisy accuracy |
|---|---|---|---|
| HOG + SVM | **0.7417** | **0.6667** | **0.2500** |
| SIFT + SVM | **0.7083** | **0.4750** | **0.7167** |

Drop compared with clean (in accuracy points):

| Model | Drop under rotation | Drop under noise |
|---|---|---|
| HOG + SVM | 0.0750 | 0.4917 |
| SIFT + SVM | 0.2333 | -0.0083 |

### Q5. Most confused classes (validation confusion matrices)

| Model | Most confused pair | Mistakes |
|---|---|---|
| HOG + SVM | eosinophil <-> neutrophil | 5 + 9 = 14 |
| SIFT + SVM | eosinophil <-> monocyte | 11 + 9 = 20 |

Eosinophil has the lowest F1 for both models, and platelet is almost perfectly recognised (F1 0.984 for both).

---

## 6. Discussion

What the numbers show:

- **Clean images:** the two pipelines are close (HOG 0.7417, SIFT 0.7083 on test; 0.7083 and 0.7000 on validation).
- **Rotation:** HOG lost 0.0750 and SIFT lost 0.2333, so HOG was more robust to a 20 degree rotation.
- **Noise:** HOG fell to 0.2500, which is the chance level for four balanced classes. SIFT stayed at 0.7167, a change of -0.0083. SIFT was far more robust to noise.
- **No single winner:** HOG handles rotation better, SIFT handles noise much better.

Possible explanations (hypotheses, not tested in the notebook):

- The grid SIFT used here fixes each keypoint angle at 0 and uses fixed positions, so it has no built-in rotation invariance. This is consistent with its larger drop under rotation.
- Noise adds many small random gradients in all directions. HOG builds histograms of gradient directions in small 8 x 8 cells and normalises per block, so noise can strongly disturb its features. SIFT smooths the image internally and describes larger 32 x 32 patches, which may make it less sensitive.
- Eosinophils are separated from neutrophils and monocytes partly by stain colour. Grayscale removes colour, leaving shape and brightness, which makes similar cell types harder to tell apart.

---

## 7. Limitations

- **Small test set:** 120 images, so one image equals about 0.83 percentage points. The few-point gaps on clean data are within chance. The large drops (HOG under noise, SIFT under rotation) are much bigger than that.
- **One seed and one split**, no repeated runs or cross-validation.
- **C selection:** for HOG all three C values gave the same validation accuracy, so C = 0.1 was chosen by the tie rule.
- **Grid SIFT has angle fixed at 0**, so it is not rotation invariant (unlike standard SIFT with orientation estimation).
- **Rotation leaves black corners**, which are new content for both models.
- **Noise is converted back to `uint8`**, because SIFT needs 8-bit images, so values are rounded slightly.
- `StandardScaler` was added to the SVM pipeline for stable training. It is fitted on training data only.

---

## 8. AI usage

AI tools were used for help with the code and explanations. The code was read, run and checked in the notebook.

| Prompt (one line) | What I changed | How I checked the result |
|---|---|---|
| "Solve my HOG vs SIFT lab mid with code and explanation" | Put my registration number FA23-BAI-014, read every line, wrote my own text answers | Printed shapes and feature lengths (8100 and 2048) and matched them with hand calculation; looked at HOG pictures and keypoint drawing |

---

*Course: Computer Vision Lab | Dataset: [BloodMNIST / MedMNIST](https://medmnist.com/)*
