Here is a clean, well-formatted summary and breakdown of your Computer Vision assignment submission, structured with clear **bold headings**.

---

**PROJECT OVERVIEW**
This project presents an automated Computer Vision pipeline designed to detect skin lesion boundaries in dermoscopy images. By combining classical image preprocessing, edge detection algorithms, contour extraction, and morphological operations, the pipeline segments the lesion region and quantifies key geometric properties.

---

**PIPELINE ARCHITECTURE**

1. **Input:** Original RGB Dermoscopy Image
2. **Grayscale Conversion:** Simplifies color channels into intensity values.
3. **Noise Reduction:** Smoothes image noise using Gaussian filtering.
4. **Edge Detection:** Extracts preliminary boundaries via the Canny algorithm.
5. **Lesion Segmentation:** Applies contour detection and morphological operations to generate a binary mask.
6. **Feature Quantification:** Calculates Lesion Area and Perimeter.

---

**KEY TASKS PERFORMED**

* **Dataset Selection:** Sampled 5 dermoscopy images from the melanocytic nevi category of the HAM10000 dataset via Hugging Face (`marmal88/skin_cancer`).
* **Smoothing Filter Comparison:** Analyzed and evaluated original, Average, Gaussian, and Median filters.
* **Edge Detection Analysis:** Compared Sobel edge detection with Canny edge detection across three distinct threshold ranges:
* Low Threshold: `50–100`
* Medium Threshold: `100–200`
* High Threshold: `150–250`


* **Mask Generation:** Transformed Canny edges into continuous lesion masks using contour extraction and morphological closing/filling.
* **Quantitative Evaluation:** Computed Area, Perimeter, and Boundary Strength. Evaluated performance against reference masks using Intersection over Union (IoU), Noise Handling, and Edge Quality.
* **Data Export:** Automated the export of quantitative metrics and comparative evaluations into structured CSV files.

---

**TECHNOLOGIES & DEPENDENCIES**

* **Programming Language:** Python 3
* **Image Processing:** OpenCV (`cv2`), NumPy
* **Data Handling & Export:** Pandas
* **Data Visualization:** Matplotlib
* **Environment:** Google Colab

---

**EXECUTION & OUTPUTS**

* **Execution Steps:** Open the notebook in Google Colab and run all cells sequentially. The dataset loads directly from Hugging Face without requiring manual file uploads.
* **Generated CSV Reports:**
* `results_table.csv`: Contains individual lesion metrics (Area, Perimeter, Boundary Strength).
* `final_comparison.csv`: Details comparative evaluation across filter types, threshold parameters, and IoU scores against baseline reference masks.



---

**CONCLUSION & INSIGHTS**
This experiment illustrates the efficacy and sensitivity of classical computer vision techniques for biomedical image segmentation. Proper noise suppression via Gaussian filtering is critical prior to edge detection, as un-smoothed noise creates false edges. Furthermore, parameter selection in Canny edge detection (specifically threshold tuning) directly impacts the accuracy of contour extraction and downstream IoU metric scores.
