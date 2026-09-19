# Lab 02 – Image Filtering and Transfer Learning

## Introduction

In this lab, we worked on the HAM10000/ISIC skin lesion dataset. The main purpose was to check how different image filters affect the performance of deep learning models.

## Dataset

The dataset contains 9 different skin lesion classes:

- Actinic keratosis
- Basal cell carcinoma
- Dermatofibroma
- Melanoma
- Nevus
- Pigmented benign keratosis
- Seborrheic keratosis
- Squamous cell carcinoma
- Vascular lesion

## Models Used

We used three pretrained models:

- VGG16
- ResNet18
- ResNet50

## Filters Used

The images were tested with:

- No Filter
- Average Filter
- Gaussian Filter
- Median Filter
- Sharpening Filter
- Sobel Filter

Each model was tested with these different filters and the results were compared.

## Evaluation

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- Macro-F1
- Balanced Accuracy
- AUC

Confusion matrices, classification reports, and training graphs were also generated.

## Conclusion

This lab helped us understand how different image filters can affect the performance of pretrained deep learning models in skin lesion classification. The complete code and results are available in the notebook.
