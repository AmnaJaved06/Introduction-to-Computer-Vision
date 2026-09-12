# Lab 1 – Skin Cancer Image Classification

**Course:** Introduction to Computer Vision

## Overview

This lab focuses on **skin cancer image classification** using Deep Learning and Machine Learning techniques. Different models are implemented and compared to evaluate their performance on image classification.

## Dataset

The **Skin Cancer ISIC – 9 Classes** dataset is used in this lab.

**Kaggle Dataset:** [Skin Cancer ISIC – 9 Classes](https://www.kaggle.com/datasets/nodoubttome/skin-cancer9-classesisic)

The dataset contains **9 different skin lesion classes**.

## Models Used

### Deep Learning

* ResNet50
* VGG16
* ResNet18
* EfficientNet-B0

### Machine Learning

* Logistic Regression
* Decision Tree
* Random Forest
* KNN
* Linear SVM
* RBF-SVM
* XGBoost

## Methodology

The images are preprocessed and resized before classification. Pretrained CNN models are used for image classification. **ResNet18** is also used as a feature extractor, and the extracted features are given to different Machine Learning classifiers.

The models are evaluated using **Accuracy, Precision, Recall, and F1-Score**. Model efficiency is also compared using **parameters, FLOPs, and inference time**.

## Results

The performance and efficiency of the implemented models are presented in **Tables 1, 2, and 3**, allowing comparison between the Deep Learning and Machine Learning approaches.

## Tools & Libraries

* Python
* PyTorch
* Torchvision
* Scikit-learn
* XGBoost
* NumPy
* Pandas
* TIMM
* THOP
* Torchinfo

## How to Run

Install the required libraries:

```bash
pip install timm thop scikit-learn xgboost torchinfo
```

Then open and run:

```text
CV_BAI_014_LAB1.ipynb
```

The dataset is downloaded using **KaggleHub** as implemented in the notebook.

## Conclusion

This lab provides practical experience in **image classification** and demonstrates how Deep Learning and Machine Learning models can be used and compared for skin lesion classification.

> **Note:** This lab is for educational purposes only and is not intended for medical diagnosis.

