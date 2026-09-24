# Lab 03 - Edge Detection Techniques and Classification Performance

## Introduction

This lab focuses on applying different edge detection techniques to skin lesion images from the HAM10000 dataset. The main purpose is to study how edge detection and image preprocessing affect image classification performance.

## Objectives

- Apply different edge detection techniques.
- Compare Sobel, Prewitt, Laplacian, LoG, and Canny methods.
- Study the effect of noise on edge detection.
- Analyze different Canny parameters.
- Compare classification performance on raw, filtered, and edge images.
- Evaluate the results using Accuracy, Precision, Recall, and F1-score.

## Dataset

The experiments use selected images from the HAM10000 skin lesion dataset.

The selected classes in this lab are:

- AKIEC
- BCC
- MEL

## Methodology

The experiment was performed in several steps:

1. Original skin lesion images were selected.
2. Different edge detection techniques were applied.
3. The effect of noise was analyzed.
4. Canny edge detection parameters were compared.
5. Raw, filtered, and edge images were used for classification.
6. Classification results were evaluated using different performance metrics.
7. Confusion matrices and comparison charts were generated.

## Edge Detection Techniques

The following techniques were tested:

- Sobel
- Prewitt
- Laplacian
- Laplacian of Gaussian (LoG)
- Canny

## Classification Models

The following models were evaluated:

- Support Vector Machine (SVM)
- Random Forest
- K-Nearest Neighbors (KNN)
- CNN Model 1
- CNN Model 2

## Evaluation Metrics

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- Training Time
- Inference Time

## Results

The experiment compared three types of images:

- Raw Images
- Filtered Images
- Edge Images

Confusion matrices were generated for the selected best-performing model. Bar charts were also used to compare classification metrics.

In this experiment, KNN was selected as the best-performing model based on the edge-image accuracy used in Task 6.

## Discussion

The experiment shows that image preprocessing and edge detection can affect classification performance. The results from raw, filtered, and edge images were compared to observe whether edge information was useful for classification.

The experiment used a small selected set of images, so the results should be considered specific to this experiment and not as a general performance measure for the HAM10000 dataset.

## Conclusion

This lab provided practical experience with edge detection and image classification. Different edge detection techniques were compared, and the effect of preprocessing on classification performance was analyzed using standard evaluation metrics.

## Tools and Libraries

- Python
- OpenCV
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- TensorFlow / Keras
- Google Colab

## References

- HAM10000 Skin Lesion Dataset
- OpenCV Documentation
- Scikit-learn Documentation
- TensorFlow / Keras Documentation
- Lab 03 instructions provided by the instructor
