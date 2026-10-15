# 🏛️ Landmark Recognition Using SIFT

A computer vision project that recognizes famous landmarks in images using the **Scale-Invariant Feature Transform (SIFT)** algorithm, feature matching, and RANSAC-based geometric verification.

## 📌 Project Overview

This project identifies landmarks by extracting distinctive visual features from reference images and comparing them with a query image. It also estimates the landmark's location within the query image and evaluates recognition performance under different image conditions.

## 🎯 Objectives

- Build an image database of famous landmarks.
- Detect keypoints and extract SIFT descriptors.
- Match image features to identify landmarks.
- Localize recognized landmarks using homography.
- Reject unknown landmarks when matching evidence is insufficient.
- Evaluate recognition performance under different conditions.

## 🏰 Landmarks

The project uses five famous landmarks:

- Eiffel Tower
- Taj Mahal
- Colosseum
- Statue of Liberty
- Great Wall of China

Images of the Sydney Opera House are used to test unknown-landmark rejection.

## ✨ Key Features

- **SIFT Feature Extraction:** Detects distinctive keypoints and extracts feature descriptors.
- **Feature Matching:** Uses a Brute-Force matcher and Lowe's Ratio Test to identify reliable matches.
- **Landmark Recognition:** Ranks candidate landmarks based on matching evidence.
- **Landmark Localization:** Uses RANSAC homography to estimate and visualize the landmark's position.
- **Unknown Landmark Detection:** Rejects queries that do not meet the recognition thresholds.
- **Performance Evaluation:** Tests recognition under rotation, resizing, brightness changes, viewpoint variations, and unknown-landmark conditions.
- **Visual Results:** Generates keypoint visualizations, feature matches, localization results, and evaluation charts.

## 🛠️ Technologies Used

- Python
- OpenCV
- SIFT Algorithm
- NumPy
- Pandas
- Matplotlib
- Jupyter Notebook / Google Colab

## ⚙️ How It Works

1. Collect reference images for each landmark.
2. Convert images to grayscale.
3. Detect keypoints and extract SIFT descriptors.
4. Extract features from the query image.
5. Match query descriptors with reference descriptors.
6. Apply Lowe's Ratio Test to filter unreliable matches.
7. Use RANSAC homography for geometric verification.
8. Recognize and localize the landmark or reject the query.
9. Evaluate the results using performance metrics and visualizations.

## 🚀 Getting Started

### Run on Google Colab

1. Download or clone this repository.
2. Open `LAB_6_Assignment_2.ipynb` in Google Colab.
3. Run the notebook cells in order.
4. Allow the notebook to download and prepare the landmark dataset.
5. View the recognition results, feature matches, localization visualizations, and evaluation reports.

**Note:** An internet connection is required to download the default dataset from Wikimedia Commons.

## 📂 Project Structure

```text
Landmark-Recognition-SIFT/
│
├── LAB_6_Assignment_2.ipynb
├── README.md
│
└── landmark_lab/
    ├── landmark_db/
    │   ├── Eiffel_Tower/
    │   ├── Taj_Mahal/
    │   ├── Colosseum/
    │   ├── Statue_of_Liberty/
    │   └── Great_Wall_of_China/
    │
    ├── queries/
    │   ├── holdout/
    │   └── unknown/
    │
    └── outputs/
        ├── sift_descriptor_database.npz
        └── screenshots/
```

*The dataset and output folders are generated when the notebook runs.*

## 📊 Evaluation

The project evaluates recognition performance using:

- Correct recognition rate
- Reliable feature matches
- RANSAC inlier counts
- Unknown-landmark rejection
- Confusion matrix
- Threshold sensitivity analysis
- Keypoint detection statistics
- Processing time

## 🎓 Project Purpose

This project demonstrates the practical application of classical computer vision techniques for image feature extraction, object recognition, image matching, and geometric localization.



Artificial Intelligence Student  
COMSATS University Islamabad
