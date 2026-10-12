# Lab 05 — HOG-Based Industrial Defect Detection & Classification

## Overview
This lab focuses on detecting and classifying steel surface defects using computer vision and machine learning. Histogram of Oriented Gradients (HOG) is used to extract image features, which are then provided to machine-learning classifiers for defect classification.

## Objectives
- Understand and explore the steel surface defect dataset.
- Preprocess images using grayscale conversion and resizing.
- Extract image features using HOG.
- Experiment with different HOG parameters.
- Train and compare machine-learning classifiers.
- Evaluate model performance using classification reports and confusion matrices.
- Test model robustness under noise, blur, brightness changes, and rotation.
- Develop a sliding-window defect inspection system.

## Dataset
This lab uses the NEU Surface Defect Database (NEU-DET), containing six types of steel surface defects:

1. Crazing
2. Inclusion
3. Patches
4. Pitted Surface
5. Rolled-in Scale
6. Scratches

The notebook uses image annotations to prepare defective image patches and samples candidate non-defective patches from other image regions.

## Tools and Libraries
- Python
- OpenCV
- NumPy
- Pandas
- Matplotlib
- Seaborn
- scikit-image
- scikit-learn
- joblib
- kagglehub

## Methodology
1. Dataset loading and inspection.
2. Image preprocessing and resizing.
3. Defective and candidate non-defective patch preparation.
4. HOG feature extraction.
5. HOG parameter comparison.
6. Machine-learning model training.
7. Model evaluation using classification metrics.
8. Robustness testing under different image conditions.
9. Sliding-window defect detection and quality inspection.
10. Saving trained models and experimental results.

## How to Run
1. Open `Lab05_HOG_Defect_Detection.ipynb` in Google Colab or Jupyter Notebook.
2. Run the installation and setup cells.
3. Download or upload the required dataset.
4. Execute the notebook cells in order.
5. Review the generated plots, classification results, confusion matrices, and output files.

## Expected Outputs
- Dataset summaries
- HOG feature visualizations
- HOG parameter comparison
- Classification reports
- Confusion matrices
- Robustness testing results
- Defect probability maps
- ACCEPT/REJECT quality inspection results
- Saved models and experimental outputs

Actual performance results depend on executing the notebook.

## Limitations
- Model performance depends on dataset quality and image annotations.
- Sampled non-defective patches may not represent completely defect-free industrial products.
- The inspection system is an educational demonstration and is not a certified industrial inspection solution.

## Conclusion
This lab demonstrates how HOG feature extraction and machine-learning classifiers can be used for steel surface defect detection. It also explores model evaluation, robustness testing, and automated quality inspection.
