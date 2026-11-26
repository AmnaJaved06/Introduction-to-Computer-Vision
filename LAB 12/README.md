# CNN Feature Visualization, Parameter Audit & Critical Analysis

A small lab Task that runs on a normal laptop or free Colab. It shows what a Convolutional Neural Network does to an image: five hand-made 3 x 3 filters are pushed through convolution, ReLU, max pooling and average pooling, two small CNN architectures are audited by hand for their trainable parameters, and five analysis questions are answered with evidence from the generated feature maps.

The full code, figures and written answers are in `CNN_Visualization_Lab_Assignment.ipynb`. Every number in this README is copied from that notebook's outputs, except the Keras parameter counts, which appear when the notebook is run in Colab (see Section 5, Part II).

## Table of contents

1. Settings
2. Project structure
3. How to run
4. Method
5. Results
6. Discussion
7. Limitations
8. AI usage

## 1. Settings

| Item | Value |
|---|---|
| Input image | Reference building photo (Figure 2 of the assignment), grayscale, 286 x 512, scaled to 0 to 1 |
| Kernels | Sobel-X, Sobel-Y, Sharpen, Box blur (1/9), Emboss (all 3 x 3) |
| Convolution | Cross-correlation, stride 1, "valid" padding |
| Pooling | 2 x 2, stride 2 (max and average), applied to the post-ReLU map |
| Random seed | 42 |
| Models | Input 32 x 32 x 3 -> Conv(5) -> Pool -> Conv(10) -> Pool -> Global Average Pooling -> Dense(10) |
| Model A | 3 x 3 kernels, Max pooling |
| Model B | 5 x 5 kernels, Average pooling |
| Padding in models | same |

## 2. Project structure

```
.
├── README.md
├── CNN_Visualization_Lab_Assignment.ipynb   # executed notebook with all outputs
└── images/                                  # figures taken from the notebook outputs
    ├── q1_input_image.png
    ├── q1_kernels.png
    ├── q1_grid_5x4.png
    ├── q2_relu_blackout.png
    ├── q2_activation_comparison.png
    ├── q3_pooling_comparison.png
    └── q3_ink_on_white.png
```

## 3. How to run

```bash
pip install numpy scipy pandas matplotlib pillow tensorflow jupyter
jupyter notebook CNN_Visualization_Lab_Assignment.ipynb
```

Run all cells from top to bottom (in Colab: Runtime -> Run all). The reference image is embedded in the notebook, so no download is needed. To use your own picture, set `USE_OWN_IMAGE = True` (Colab upload). TensorFlow is only needed for the Keras model cells in Part II.

Libraries used: numpy, scipy, pandas, matplotlib, pillow, tensorflow/keras.

## 4. Method

### Part I. Filter pipeline
- Convolution, ReLU and both poolings are written from scratch in NumPy. The convolution is checked against `scipy.signal.correlate2d` with an assertion.
- Each of the five kernels produces four stages: **A** raw convolution, **B** post-ReLU, **C** max pool, **D** average pool.
- A 5 x 4 grid shows all stages. Stage A uses a diverging colormap centred on 0. Stages B, C, D share one grey scale per row (0 to the maximum of Stage B), and every panel has a colorbar.
- A synthetic edge test proves which structures each Sobel kernel responds to.

### Part II. Parameter audit
Formulas used:

```
Conv params  = (Kh x Kw x Cin + 1) x Cout
Dense params = (inputs + 1) x outputs
```

Padding, ReLU, pooling and Global Average Pooling have 0 trainable parameters. The hand calculation is done before `model.summary()` is called, and an assertion compares it with Keras' `count_params()`.

### Part III. Critical questions
Each answer is preceded by a code cell that produces its evidence (ReLU blackout overlay, Leaky ReLU and Sigmoid comparison, pooling zoom, an ink-on-white experiment, a numerical proof for operation order, and a filter-collapse demonstration).

## 5. Results

### Part I. Feature maps

Input image and kernels

![Input](images/q1_input_image.png)
![Kernels](images/q1_kernels.png)

Feature map sizes: Conv/ReLU 284 x 510, both poolings 142 x 255.

5 x 4 grid (5 filters x 4 stages)

![Grid](images/q1_grid_5x4.png)

| Filter | Raw min / max | % negative in A | % zero in B (blackout) | Max-pool peak | Avg-pool peak | Peak kept by avg |
|---|---|---|---|---|---|---|
| Sobel-X | -3.710 / 3.878 | 48.79 | 53.13 | 3.878 | 3.151 | 0.812 |
| Sobel-Y | -3.290 / 3.741 | 41.92 | 45.97 | 3.741 | 3.101 | 0.829 |
| Sharpen | -1.361 / 2.447 | 18.13 | 18.58 | 2.447 | 1.634 | 0.668 |
| Box blur | 0.000 / 0.957 | 0.00 | 0.18 | 0.957 | 0.954 | 0.997 |
| Emboss | -2.584 / 3.612 | 23.61 | 24.25 | 3.612 | 3.032 | 0.840 |

### Part II. Parameter audit

| Model | Kernel | Pooling | Conv1 | Conv2 | GAP | Dense | Manual count | Code output |
|---|---|---|---|---|---|---|---|---|
| A (Standard) | 3 x 3 | Max | (3x3x3+1)x5 = 140 | (3x3x5+1)x10 = 460 | 0 | (10+1)x10 = 110 | **710** | shown when run in Colab |
| B (Large Kernel) | 5 x 5 | Average | (5x5x3+1)x5 = 380 | (5x5x5+1)x10 = 1260 | 0 | (10+1)x10 = 110 | **1750** | shown when run in Colab |

Shape trace: 32x32x3 -> 32x32x5 -> 16x16x5 -> 16x16x10 -> 8x8x10 -> 10 -> 10.

### Part III. Questions

**Q1. ReLU blackout and alternatives**

![Blackout](images/q2_relu_blackout.png)
![Activations](images/q2_activation_comparison.png)

| Activation | Min | Max | % pixels exactly 0 | % pixels below 0 |
|---|---|---|---|---|
| Raw (Stage A) | -3.710 | 3.878 | 4.35 | 48.79 |
| ReLU | 0.000 | 3.878 | 53.13 | 0.00 |
| Leaky ReLU (alpha 0.1) | -0.371 | 3.878 | 4.35 | 48.79 |
| Sigmoid | 0.024 | 0.980 | 0.00 | 0.00 |

Sobel-X is black after ReLU where brightness falls from left to right (right-hand edges of bright columns). Sobel-Y is black where brightness falls from top to bottom (underside of bright slabs).

**Q2. Max vs average pooling**

![Pooling](images/q3_pooling_comparison.png)

| Filter | % pixels above 50% of peak, max pool | % pixels above 50% of peak, avg pool |
|---|---|---|
| Sharpen | 12.37 | 0.73 |
| Sobel-X | 5.31 | 1.37 |
| Sobel-Y | 3.84 | 1.67 |
| Emboss | 10.97 | 4.41 |
| Box blur | 39.92 | 35.36 |

Dark ink on a white page (synthetic text image):

![Ink](images/q3_ink_on_white.png)

| Image | % ink pixels (value below 0.5) |
|---|---|
| Original | 5.24 |
| Max pool | 2.70 |
| Average pool | 4.92 |
| Max pool on inverted image | 7.78 |

**Q3. Parameter growth and receptive field**
- Total parameters rise from 710 to 1750: (1750 - 710) / 710 = **146.48 %** (conv layers only: 600 to 1640, 173.33 %).
- Two stacked 3 x 3 layers give the 5 x 5 receptive field.
- With C input and C output channels: 5 x 5 costs 25C^2 + C, two 3 x 3 cost 18C^2 + 2C. The weights shrink by 28 %. For C = 5: 630 vs 460, a saving of 170.
- Inside Model B the saving depends on the middle width: Conv1 (3 -> 5 -> 5) saves 10, Conv2 as 5 -> 5 -> 10 saves 570, but 5 -> 10 -> 10 costs 110 more.

**Q4. Order of ReLU and max pooling**
- Conv -> ReLU -> MaxPool and Conv -> MaxPool -> ReLU give identical output, because ReLU is monotonic: max(ReLU(x_i)) = ReLU(max(x_i)).
- Patch [[-3, 2], [-1, -5]]: both orders give 2. All-negative patch [[-3, -2], [-1, -5]]: both give 0.
- Checked on 100,000 random patches (maximum difference 0.0) and on all five real feature maps (identical).
- Pooling first needs 4x fewer ReLU operations (1,280 instead of 5,120 in the first block of Model A).
- It does not hold for average pooling: the first patch gives 0.5 (ReLU first) vs 0.0 (average first).

**Q5. Filter collapse**

| Cause | Demonstration | Fix |
|---|---|---|
| Symmetric or poor initialization | Constant init: mean map correlation 1.000. He-normal init: 0.341 | He (Kaiming) random initialization, standardised inputs, BatchNorm |
| Dead ReLUs from high learning rate or large negative bias | Bias 0: 92.4 % active. Bias -2: 0.0 % active | Lower learning rate (Adam 1e-3 or less, warm-up, gradient clipping), Leaky ReLU, small or zero bias |

## 6. Discussion

What the numbers show:

- **Edge filters** (Sobel-X and Sobel-Y) lose about half of their map to ReLU (53.1 % and 46.0 % black), because each edge has one positive and one negative side and ReLU keeps only one.
- **Sharpen** is hit hardest by average pooling (only 66.8 % of its peak kept, and 12.37 % of pixels above half-peak fall to 0.73 %), because its thin, bright lines get mixed with dark neighbours.
- **Box blur** is almost unchanged by the choice of pooling (99.7 % of peak kept), which shows the effect belongs to high-frequency detail.
- **Model B** has 2.46x the parameters of Model A (146.48 % more) while giving the same output size, so it is a costly way to get a 5 x 5 view.
- **Ink on white** is a case where average pooling is better than max pooling, because the background is the bright value and wins every max-pool window.

Possible explanations (hypotheses, shown by the experiments but not tested further):

- Sobel responses are signed differences that sum to zero, so a flat area gives exactly 0 and the reverse edge polarity is negative.
- Sigmoid squashes everything into (0, 1) with flat areas at 0.5, so the map has no black background, but strong edges are compressed.

## 7. Limitations

- One image, one set of kernels, no repeated runs, so the percentages describe this photo only.
- The two CNNs are built and counted but **not trained**. No accuracy comparison between Model A and Model B is made.
- The Keras `model.summary()` output is not stored in this README because it needs TensorFlow. Run the notebook in Colab to get it and check it against the hand counts (710 and 1750).
- Sobel-X is called the "horizontal edge detector" in the assignment sheet, but it responds mostly to vertical structures. The notebook keeps the sheet's label and shows the real behaviour.
- Pooling crops odd borders, and "valid" convolution shrinks the map by 2 pixels per side pair.
- The ink-on-white experiment uses a small synthetic text image, not a scanned document.

## 8. AI usage

AI tools were allowed by the assignment for the Python implementation, but the code alone earns no credit without the visual evidence and the viva. The notebook was prepared with the help of Claude (Anthropic).

| Prompt (one line) | What I changed | How I checked the result |
|---|---|---|
| "Make a Colab notebook for this lab assignment, run all code, with headings" | _Write here what you changed, for example your name, image, or your own wording of the answers_ | Convolution compared with `scipy.signal.correlate2d`; synthetic edge test for the Sobel signs; parameter counts matched by hand; Keras counts checked by an assertion in Colab; figures inspected |

Course: Deep Learning & Computer Vision Lab
