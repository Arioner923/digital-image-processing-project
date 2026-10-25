# Digital Image Processing Project Report

## 1. Introduction

This project is a reimplementation of a university assignment developed for the **Digital Image Processing Laboratory**.

The objective of the project is to apply fundamental digital image processing techniques using Python and Jupyter Notebook. The implementation follows the scope and requirements of the original university assignment and focuses on classical spatial-domain and frequency-domain image processing methods.

Two images provided with the assignment are used throughout the project:

- `aloe-plant-pot.jpg`
- `noise_world_map.png`

The implemented topics include:

- RGB and HSV color-space analysis
- Grayscale and binary image conversion
- Histogram analysis
- Threshold-based image segmentation
- Geometric image transformations
- Gaussian noise generation
- Manual spatial filtering
- Convolution using SciPy
- Connected-component labeling
- 8-connectivity
- Fourier-transform analysis
- Periodic-noise removal using a Butterworth notch-reject filter

---

## 2. Development Environment

The project is implemented in Python using a Jupyter Notebook.

The main libraries used are:

- **NumPy** — numerical calculations, arrays, filtering operations, and Fourier transforms
- **Matplotlib** — image visualization, plots, histograms, and result comparison
- **Pillow** — image loading, conversion, resizing, rotation, and saving
- **SciPy** — convolution, connected-component labeling, filtering, and image-processing utilities

The notebook uses relative paths so that it can be executed directly from the repository without machine-specific absolute paths.

A fixed random seed is also used during Gaussian-noise generation to make the experiment reproducible.

---

## 3. Input Images

### 3.1 Aloe Plant Image

The image:

```text
aloe-plant-pot.jpg
```

contains an aloe plant inside a pot on a bright background.

It is used for:

- RGB channel analysis
- HSV conversion
- grayscale conversion
- binary conversion
- histogram analysis
- pot segmentation
- geometric transformations
- Gaussian-noise generation
- mean filtering

---

### 3.2 Noisy World Map

The image:

```text
noise_world_map.png
```

contains a world map affected by visible periodic interference.

It is used for:

- threshold-based land segmentation
- connected-component labeling
- South America extraction
- Fourier-spectrum analysis
- periodic-noise removal

---

# 4. Question 1 — Color Spaces and Image Conversion

## 4.1 RGB Channel Analysis

The aloe image is initially represented in the RGB color space.

An RGB image consists of three color channels:

- Red
- Green
- Blue

Each channel stores the intensity contribution of the corresponding color component.

The three channels are extracted independently and displayed alongside the original RGB image.

This visualization demonstrates how different parts of the image contribute differently to the red, green, and blue components.

---

## 4.2 HSV Color Space

The RGB image is converted to the HSV color space.

HSV separates image information into:

- **Hue** — dominant color or wavelength information
- **Saturation** — color purity or intensity
- **Value** — brightness

Unlike RGB, HSV separates color information more clearly from brightness information.

The Hue, Saturation, and Value channels are displayed independently.

---

## 4.3 Grayscale Conversion

The RGB image is converted to grayscale.

A grayscale image contains a single intensity value for every pixel instead of three separate RGB values.

The grayscale image is reused in later stages of the project, including:

- histogram analysis
- segmentation
- geometric transformations
- noise generation
- spatial filtering

---

## 4.4 Binary Image Conversion

The grayscale image is converted into a binary representation using a global intensity threshold.

The binary image contains only two pixel classes:

- black
- white

For an intensity threshold \(T\), the conversion can be represented as:

\[
B(x,y)=
\begin{cases}
255, & I(x,y) \geq T \\
0, & I(x,y) < T
\end{cases}
\]

where \(I(x,y)\) is the grayscale intensity.

This operation demonstrates one of the simplest forms of image segmentation.

---

# 5. Question 2 — Histogram-Based Pot Segmentation

## 5.1 Grayscale Histogram

A histogram of the grayscale aloe image is calculated.

The histogram represents the distribution of intensity values from:

\[
0 \rightarrow 255
\]

where:

- lower values correspond to darker pixels
- higher values correspond to brighter pixels

The histogram is used to investigate which intensity ranges correspond approximately to the pot and the bright background.

---

## 5.2 Threshold Selection

The pot is considerably darker than the white background.

Therefore, an intensity range is selected to generate an initial segmentation mask.

The selected thresholds are explicitly displayed on the grayscale histogram.

Using a range instead of a single threshold provides greater control over which grayscale intensities are considered part of the pot.

---

## 5.3 Pot Mask

The threshold operation can also select dark pixels from other areas of the image, including parts of the plant and shadows.

Since the pot is located in the lower region of the image, the threshold mask is restricted to that area.

Basic binary morphology is then used to:

- close small gaps
- remove isolated pixels
- improve the continuity of the selected object

Connected-component labeling is subsequently applied to the candidate regions.

The largest suitable connected region is retained as the final pot mask.

---

## 5.4 Pot Extraction

The final segmentation mask is applied to the original RGB image.

Pixels belonging to the detected pot retain their original RGB values.

All other pixels are replaced with white.

The final result therefore displays the pot isolated against a white background.

The generated result is stored as:

```text
results/segmentation/pot_segmentation.png
```

---

# 6. Question 3 — Geometric Image Transformations

Three geometric transformations are applied to the grayscale aloe image.

---

## 6.1 Image Transposition

Image transposition exchanges the row and column axes.

For an image matrix:

\[
I(x,y)
\]

the transposed image becomes:

\[
I_T(x,y)=I(y,x)
\]

The resulting image is stored as:

```text
results/transformations/grayscale_transposed.png
```

---

## 6.2 Image Resizing

The grayscale image is resized to:

```text
300 × 255 pixels
```

Pillow is used for the resizing operation.

The generated result is stored as:

```text
results/transformations/grayscale_resized.png
```

---

## 6.3 Image Rotation

The grayscale image is rotated:

```text
45° counter-clockwise
```

The image canvas is expanded during rotation so that the transformed image is not unnecessarily cropped.

The generated result is stored as:

```text
results/transformations/grayscale_rotated_45.png
```

---

## 6.4 Transformation Comparison

The original grayscale image and all three transformed images are displayed together.

This makes it possible to visually compare:

- original image
- transpose
- resize
- rotation

---

# 7. Question 4 — Gaussian Noise

Gaussian noise is added to the grayscale aloe image.

The generated random noise follows a normal distribution:

\[
N \sim \mathcal{N}(\mu,\sigma^2)
\]

with:

\[
\mu = 0
\]

and:

\[
\sigma = 50
\]

The noisy image is therefore calculated as:

\[
I_{noisy}(x,y)=I(x,y)+N(x,y)
\]

Because the resulting values may exceed the valid 8-bit image range, they are clipped to:

\[
[0,255]
\]

A fixed random seed is used so that repeated notebook execution produces the same noise pattern.

The generated noisy image is stored as:

```text
results/filtering/gaussian_noise_std50.png
```

---

# 8. Question 5 — Manual 5×5 Mean Filter

The assignment refers to a 5×5 mean filter and subsequently requires the same mask to be reused with a convolution function.

For this implementation, the filter is therefore treated as a **5×5 arithmetic averaging filter**.

The filter kernel is:

\[
H=
\frac{1}{25}
\begin{bmatrix}
1&1&1&1&1\\
1&1&1&1&1\\
1&1&1&1&1\\
1&1&1&1&1\\
1&1&1&1&1
\end{bmatrix}
\]

Each output pixel is calculated from the average value of its local 5×5 neighborhood.

---

## 8.1 Manual Filtering Implementation

The filtering operation is implemented manually without using a ready-made convolution function.

For every pixel:

1. A 5×5 neighborhood is selected.
2. The neighborhood is multiplied element-wise by the filter kernel.
3. The resulting values are summed.
4. The calculated value is stored in the output image.

Padding is applied at the image boundaries so that the filtered image retains the same dimensions as the input.

This part of the project demonstrates the internal operation of spatial-domain convolution instead of relying only on a library function.

The generated result is stored as:

```text
results/filtering/manual_mean_filter_5x5.png
```

---

# 9. Question 6 — Convolution Using SciPy

The same 5×5 averaging kernel is applied to the Gaussian-noisy image using:

```python
scipy.ndimage.convolve
```

This allows the manually implemented filtering algorithm to be compared with a standard library implementation.

The generated result is stored as:

```text
results/filtering/scipy_mean_filter_5x5.png
```

---

## 9.1 Numerical Verification

The output of the manual filter and the SciPy convolution result are compared numerically.

After matching the image-boundary handling used by both implementations, the observed differences are only at floating-point precision level.

The measured values are approximately:

```text
Maximum absolute difference ≈ 1.7 × 10⁻13
Mean absolute difference    ≈ 2.6 × 10⁻14
```

These extremely small values confirm that the manual implementation produces the same filtering result as the SciPy convolution operation.

---

## 9.2 Filtering Comparison

The assignment requires the following four images to be shown together:

1. Original grayscale image
2. Gaussian-noisy image
3. Manual 5×5 filtered image
4. SciPy convolution result

The final comparison figure is stored as:

```text
results/filtering/filtering_comparison.png
```

The filtering operation clearly reduces the high-frequency Gaussian noise, although some smoothing of image details is expected when using an averaging filter.

---

# 10. Question 7 — South America Extraction

The second input image is processed in order to isolate South America.

The assignment specifically requires the use of connected-component labeling with **8-connectivity**.

---

## 10.1 Image Preprocessing

The world-map image is first converted to grayscale.

A small amount of Gaussian smoothing is applied to reduce the influence of the periodic pattern during segmentation.

This preprocessing step makes the continent boundaries more suitable for thresholding and labeling.

---

## 10.2 Binary Land Segmentation

The continents are darker than the bright background.

A grayscale threshold is therefore applied to create a binary land mask.

Small isolated regions are reduced using basic binary opening and closing operations.

---

## 10.3 8-Connectivity

With 8-connectivity, every pixel may be connected to:

- horizontal neighbors
- vertical neighbors
- diagonal neighbors

The connectivity structure is:

\[
\begin{bmatrix}
1&1&1\\
1&1&1\\
1&1&1
\end{bmatrix}
\]

This type of connectivity is useful for irregular shapes whose pixels may connect diagonally.

---

## 10.4 Connected-Component Labeling

Connected-component labeling assigns a unique integer label to every connected region in the binary image.

The implementation uses:

```python
scipy.ndimage.label
```

with an 8-connected neighborhood.

Each connected land region can then be identified independently.

---

## 10.5 South America Selection

South America is located in the lower-middle area of the supplied world-map image.

A known interior location inside South America is used to identify the corresponding connected-component label.

The entire component associated with this label is then extracted.

This keeps the final extraction based on connected-component labeling rather than directly cropping the image.

The generated result is stored as:

```text
results/segmentation/south_america_extraction.png
```

---

# 11. Question 8 — Periodic Noise Removal

The world-map image contains visible periodic interference.

Periodic noise is particularly suitable for frequency-domain analysis because repeating spatial patterns generate characteristic frequency components in the Fourier spectrum.

---

## 11.1 Two-Dimensional Fourier Transform

The grayscale map is transformed from the spatial domain:

\[
f(x,y)
\]

to the frequency domain:

\[
F(u,v)
\]

using the two-dimensional Fast Fourier Transform.

The spectrum is shifted so that the zero-frequency component is positioned at the center.

The logarithmic magnitude spectrum is displayed using:

\[
\log(1+|F(u,v)|)
\]

The logarithmic representation makes weaker frequency components easier to visualize.

The original spectrum is stored as:

```text
results/frequency_domain/original_frequency_spectrum.png
```

---

## 11.2 Periodic-Noise Frequency Detection

The useful low-frequency image information is concentrated around the center of the Fourier spectrum.

For this reason, the central low-frequency region is excluded from the periodic-noise peak search.

Local maxima outside this area are examined.

Because the Fourier transform of a real-valued image has conjugate symmetry, periodic-noise frequencies appear in symmetric pairs around the spectrum center.

The strongest suitable symmetric frequency pairs are selected for filtering.

---

## 11.3 Butterworth Notch-Reject Filter

A **Butterworth notch-reject filter** is constructed around the detected periodic-noise frequencies.

Unlike a simple low-pass filter, a notch-reject filter targets selected frequencies instead of suppressing all high-frequency image information.

For every detected periodic-noise frequency, a corresponding symmetric notch is also added.

The Butterworth response provides a smooth transition around the rejected frequencies.

Its behavior is controlled by:

- notch radius
- filter order

The resulting filter mask is stored as:

```text
results/frequency_domain/butterworth_notch_mask.png
```

---

## 11.4 Frequency-Domain Filtering

The shifted Fourier spectrum is multiplied by the Butterworth notch-reject mask:

\[
G(u,v)=H(u,v)F(u,v)
\]

where:

- \(F(u,v)\) is the original Fourier spectrum
- \(H(u,v)\) is the Butterworth filter
- \(G(u,v)\) is the filtered spectrum

The selected periodic-noise frequencies are therefore attenuated.

---

## 11.5 Image Reconstruction

After filtering:

1. the frequency spectrum is shifted back,
2. the inverse Fourier transform is applied,
3. the real part of the reconstructed signal is retained,
4. the resulting intensity values are restricted to the valid image range.

The final image shows a strong reduction of the periodic interference while retaining substantially more image detail than would be preserved by a general low-pass filter.

The final result is stored as:

```text
results/frequency_domain/world_map_denoised.png
```

---

# 12. Results

The project successfully demonstrates the requested Digital Image Processing techniques.

The most important generated results include:

### Segmentation

```text
results/segmentation/pot_segmentation.png
results/segmentation/south_america_extraction.png
```

### Geometric Transformations

```text
results/transformations/grayscale_transposed.png
results/transformations/grayscale_resized.png
results/transformations/grayscale_rotated_45.png
```

### Noise and Spatial Filtering

```text
results/filtering/gaussian_noise_std50.png
results/filtering/manual_mean_filter_5x5.png
results/filtering/scipy_mean_filter_5x5.png
results/filtering/filtering_comparison.png
```

### Frequency-Domain Processing

```text
results/frequency_domain/original_frequency_spectrum.png
results/frequency_domain/butterworth_notch_mask.png
results/frequency_domain/world_map_denoised.png
```

---

# 13. Reproducibility

The notebook is designed to execute sequentially from a clean Python kernel.

The complete implementation can be verified using:

```text
Restart Kernel → Run All
```

A fixed random seed ensures that the Gaussian-noise experiment produces reproducible results.

Relative paths are used for both input files and generated outputs.

Executing the notebook automatically recreates the main result files under the corresponding `results/` directories.

---

# 14. Project Structure

```text
digital-image-processing-project/
│
├── docs/
│   └── report.md
│
├── images/
│   ├── aloe-plant-pot.jpg
│   └── noise_world_map.png
│
├── notebooks/
│   └── digital_image_processing_project.ipynb
│
├── results/
│   ├── filtering/
│   │   ├── filtering_comparison.png
│   │   ├── gaussian_noise_std50.png
│   │   ├── manual_mean_filter_5x5.png
│   │   └── scipy_mean_filter_5x5.png
│   │
│   ├── frequency_domain/
│   │   ├── butterworth_notch_mask.png
│   │   ├── original_frequency_spectrum.png
│   │   └── world_map_denoised.png
│   │
│   ├── segmentation/
│   │   ├── pot_segmentation.png
│   │   └── south_america_extraction.png
│   │
│   └── transformations/
│       ├── grayscale_resized.png
│       ├── grayscale_rotated_45.png
│       └── grayscale_transposed.png
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

# 15. Conclusions

This project implements a range of fundamental Digital Image Processing techniques using Python.

The first part of the work focuses on basic image representations and spatial-domain operations, including:

- RGB and HSV channel analysis
- grayscale conversion
- binary thresholding
- histogram analysis
- segmentation
- geometric transformations

The second part examines image degradation and restoration through:

- Gaussian-noise generation
- manual 5×5 mean filtering
- convolution using SciPy

The manual filtering implementation was numerically compared with the SciPy implementation and produced equivalent results within floating-point precision.

Connected-component labeling with 8-connectivity was then used to identify and isolate South America from the supplied world-map image.

Finally, Fourier-domain analysis was used to investigate the periodic interference present in the map. A Butterworth notch-reject filter was constructed to suppress selected periodic-noise frequencies while preserving the remaining image information.

Overall, the project demonstrates both spatial-domain and frequency-domain approaches to classical Digital Image Processing and provides a practical implementation of the main concepts covered by the university assignment.