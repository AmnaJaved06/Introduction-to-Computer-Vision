**PROJECT OVERVIEW**

This lab implements a Computer Vision pipeline for detecting the boundary of skin lesions in dermoscopy images using image preprocessing, edge detection, contour extraction, and image analysis techniques. The main goal of this assignment is to detect the boundary of a skin lesion from dermoscopy images and analyze the detected region.

---

**PIPELINE**

Original Image → Grayscale → Gaussian Filtering → Canny Edge Detection → Lesion Boundary → Area & Perimeter

---

**TASKS PERFORMED**

* Loaded 5 dermoscopy images from the HAM10000 skin lesion dataset.
* Converted RGB images to grayscale.
* Applied different smoothing filters:
* Original
* Average
* Gaussian
* Median


* Applied Sobel edge detection.
* Applied Canny edge detection using different threshold values:
* 50–100
* 100–200
* 150–250


* Converted detected edges into a lesion mask using contour detection and morphological operations.
* Calculated:
* Lesion area
* Lesion perimeter
* Boundary strength


* Compared different filters and edge detection methods.
* Used a reference lesion mask for final comparison.
* Evaluated the results using:
* Noise Handling
* Edge Quality
* Boundary Detection / IoU


* Saved the results into CSV files.

---

**DATASET**

The notebook uses the HAM10000 skin lesion dataset through Hugging Face.
Dataset used in the notebook: **"marmal88/skin_cancer"**
The notebook automatically selects images belonging to the **nevi** (melanocytic nevi) category.

---

**TECHNOLOGIES & LIBRARIES**

* Python
* OpenCV
* NumPy
* Pandas
* Matplotlib
* Hugging Face Datasets
* Google Colab

---

**HOW TO RUN**

1. Open the notebook in Google Colab.
2. Run the cells from top to bottom.
3. The required dataset is loaded automatically from Hugging Face.
4. The notebook processes the selected dermoscopy images.
5. View the generated plots and comparison results.
6. The final CSV files are generated automatically.

---

**OUTPUT FILES**

The notebook generates:

* **"results_table.csv"**
* **"final_comparison.csv"**

These files contain the measurements and comparison results obtained from the image-processing pipeline.

---

**CONCLUSION**

This assignment demonstrates how classical computer vision techniques can be used for skin lesion boundary detection. Gaussian filtering helps reduce noise, while Canny and Sobel edge detectors identify important image boundaries. Contour detection and morphological operations are then used to obtain the lesion region and calculate its area and perimeter.

The comparison of different filters and edge detection methods shows how preprocessing and threshold selection can affect the quality of lesion boundary detection.
