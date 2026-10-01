CV Assignment 01 – Lab 04

Skin Lesion Boundary Detection Using Canny Edge Detection

This lab implements a Computer Vision pipeline for detecting the boundary of skin lesions in dermoscopy images using image preprocessing, edge detection, contour extraction, and image analysis techniques.

Project Overview

The main goal of this assignment is to detect the boundary of a skin lesion from dermoscopy images and analyze the detected region.

Pipeline

Original Image → Grayscale → Gaussian Filtering → Canny Edge Detection → Lesion Boundary → Area & Perimeter

Tasks Performed

- Loaded 5 dermoscopy images from the HAM10000 skin lesion dataset.
- Converted RGB images to grayscale.
- Applied different smoothing filters:
  - Original
  - Average
  - Gaussian
  - Median
- Applied Sobel edge detection.
- Applied Canny edge detection using different threshold values:
  - 50–100
  - 100–200
  - 150–250
- Converted detected edges into a lesion mask using contour detection and morphological operations.
- Calculated:
  - Lesion area
  - Lesion perimeter
  - Boundary strength
- Compared different filters and edge detection methods.
- Used a reference lesion mask for final comparison.
- Evaluated the results using:
  - Noise Handling
  - Edge Quality
  - Boundary Detection / IoU
- Saved the results into CSV files.

Dataset

The notebook uses the HAM10000 skin lesion dataset through Hugging Face.

Dataset used in the notebook:

"marmal88/skin_cancer"

The notebook automatically selects images belonging to the nevi (melanocytic nevi) category.

Technologies & Libraries

- Python
- OpenCV
- NumPy
- Pandas
- Matplotlib
- Hugging Face Datasets
- Google Colab

How to Run

1. Open the notebook in Google Colab.
2. Run the cells from top to bottom.
3. The required dataset is loaded automatically from Hugging Face.
4. The notebook processes the selected dermoscopy images.
5. View the generated plots and comparison results.
6. The final CSV files are generated automatically.

Output Files

The notebook generates:

- "results_table.csv"
- "final_comparison.csv"

These files contain the measurements and comparison results obtained from the image-processing pipeline.


