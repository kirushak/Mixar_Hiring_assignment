# Mixar_Hiring_assignment
This assignment focuses on understanding and implementing data preprocessing for 3D meshes, which is a fundamental step in our research workflow.

# 🧩 3D Mesh Normalization, Quantization, and Reconstruction Pipeline

### 📚 **Overview**

This project focuses on developing a complete pipeline for **3D mesh preprocessing**, including **normalization, quantization, and reconstruction**.
It explores how different normalization strategies affect geometric fidelity after quantization, and evaluates reconstruction performance across multiple meshes.

The system processes `.obj` mesh files, computes geometric statistics, normalizes and quantizes vertex coordinates, reconstructs the meshes, and measures reconstruction errors — forming a foundation for 3D data compression and geometry-aware machine learning applications.

## ⚙️ **Core Objectives**

1. Understand and visualize 3D mesh geometry.
2. Implement normalization techniques — **Min–Max** and **Unit-Sphere**.
3. Apply quantization and analyze reconstruction fidelity.
4. Reconstruct meshes via dequantization and denormalization.
5. Evaluate and visualize reconstruction errors across all methods.

## 🧠 **Pipeline Overview**

### **1️⃣ Data Understanding and Visualization**

Meshes are loaded using `trimesh`, and vertex coordinates are extracted as NumPy arrays.
For each mesh:

* Compute per-axis statistics — **min**, **max**, **mean**, **standard deviation**.
* Store results in `mesh_summary.json` for reference.
* Visualize the vertex cloud using `matplotlib` 3D scatter plots for spatial understanding.

This establishes the baseline geometric properties of the dataset.


### **2️⃣ Normalization and Quantization**

Two normalization methods were implemented:

* **Min–Max Normalization:**
  Scales each axis to the range [0,1], preserving relative axis dimensions.

* **Unit-Sphere Normalization:**
  Centers the mesh at the centroid and scales all vertices to fit within a unit sphere, ensuring rotation and translation invariance.

After normalization:

* Vertex coordinates are **quantized** into discrete bins (default = 1024).
* Quantized meshes are **dequantized** back into floating-point coordinates.
* Reconstructed meshes are saved for visual inspection.

### **3️⃣ Reconstruction and Error Evaluation**

Once meshes are reconstructed:

* The system **dequantizes and denormalizes** vertices to their original coordinate space.
* Computes **Mean Absolute Error (MAE)**, **Root Mean Squared Error (RMSE)**, and **Euclidean distance** between original and reconstructed meshes.
* Plots per-axis and overall reconstruction errors for both normalization techniques.

All numerical results are saved in `outputs/task3/` as `.csv` summaries and `.png` visualizations.

## 🧾 **Workflow Summary**

| Step  | Operation      | Description                                    |
| ----- | -------------- | ---------------------------------------------- |
| **1** | Mesh Loading   | Read `.obj` meshes, extract vertex coordinates |
| **2** | Statistics     | Compute per-axis min, max, mean, std           |
| **3** | Normalization  | Apply Min–Max and Unit-Sphere scaling          |
| **4** | Quantization   | Discretize vertices (default: 1024 bins)       |
| **5** | Reconstruction | Dequantize and denormalize coordinates         |
| **6** | Evaluation     | Compute MSE, MAE, Euclidean errors             |
| **7** | Visualization  | Generate 3D plots and comparative graphs       |


## 📊 **Key Findings & Observations**

* **Normalization Impact:**

  * Min–Max normalization yielded the **lowest average reconstruction error**, especially for axis-aligned meshes.
  * Unit-Sphere normalization provided **rotation and translation invariance**, performing more consistently under transformations.

* **Quantization Effects:**

  * Increasing quantization bins (e.g., from 64 → 1024) significantly improved reconstruction fidelity.
  * At 1024 bins, both methods produced near-lossless visual reconstruction.

* **Error Distribution:**

  * Most errors occur near **edges and high-curvature regions**, where geometric detail is concentrated.
  * Mean Euclidean error remained below **10⁻³** across most meshes.

* **Overall Insight:**

  * Combining Min–Max normalization with uniform 1024-level quantization provides an optimal trade-off between simplicity and accuracy.
  * Unit-Sphere normalization remains preferable for orientation-variant datasets.


## 📁 **Directory Structure**

```

├── meshes/                     # Input .obj meshes
├── outputs/
│   ├── task1/                  # Mesh statistics and visualizations
│   ├── task2/                  # Normalized & quantized meshes
│   ├── task3/                  # Reconstruction and error results
├── task1_visualization.ipynb
├── task2_normalization.ipynb
├── task3_reconstruction.ipynb
└── README.md
```

## ▶️ **How to Run**

### **1. Install Dependencies**

```bash
pip install numpy trimesh matplotlib pandas scipy
```

### **2. Place Input Meshes**

Put your `.obj` files inside the `meshes/` directory.

### **3. Execute Pipeline**

Run the notebook cells (or Python scripts) in order:

1. **Task 1** → `task1_visualization.ipynb`
2. **Task 2** → `task2_normalization.ipynb`
3. **Task 3** → `task3_reconstruction.ipynb`

All outputs (plots, CSVs, reconstructed meshes) will be automatically saved inside the `outputs/` folder.


## 📈 **Results Summary**

| Normalization   | Mean Euclidean Error | Invariance                            | Visual Quality                    |
| --------------- | -------------------- | ------------------------------------- | --------------------------------- |
| **Min–Max**     | Lowest (≈10⁻⁴–10⁻³)  | Moderate                              | Excellent for axis-aligned meshes |
| **Unit-Sphere** | Slightly higher      | High (rotation/translation invariant) | Stable under transformations      |

Both methods achieve visually accurate reconstructions, with minimal perceptual difference at 1024 bins.

## 🧩 **Conclusion**

The project successfully demonstrates a complete 3D mesh quantization pipeline, from geometric analysis to reconstruction evaluation.
**Min–Max normalization** achieves the best numerical accuracy, while **Unit-Sphere normalization** ensures geometric invariance.
Together, they provide valuable insight into how preprocessing strategies affect downstream 3D data compression, transmission, and learning.
