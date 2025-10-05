# 🧬 DA5401 A5 — Visualizing Data Veracity Challenges in Multi-Label Classification

**Author:** *Dikshank Sihag (DA25M009)*
**Dataset:** Yeast (Multi-label gene expression)  
**Notebook:** `Assignment5.ipynb`  
**Goal:** To explore and visualize **data veracity issues** (noisy labels, outliers, and hard-to-learn samples) using **non-linear dimensionality reduction** techniques — *t-SNE* and *Isomap*.

---

## 📘 Overview

This notebook presents an **end-to-end manifold learning workflow** on the **Yeast multi-label dataset**, combining classical data preprocessing with advanced visualization to uncover structural irregularities in high-dimensional biological data.

The focus is on identifying **data veracity challenges** that impact model performance:
- 🧩 **Noisy / Ambiguous Labels** — overlapping or mixed class samples  
- 🚨 **Outliers** — samples that deviate significantly from the main distribution  
- 🎯 **Hard-to-Learn Samples** — regions where classes are deeply entangled  

---

## ⚙️ Steps Implemented

### 1️⃣ Data Loading & Exploration
- Loaded **Yeast** dataset (2417 samples × 103 features + 14 labels).  
- Automatically handled both `.arff` and `.csv` formats for flexibility.  
- Verified dimensions and structure:  


### 2️⃣ Label Simplification
To create interpretable visualizations:
- Extracted **binary label matrix (Y)**.  
- Computed **frequency of each label** and most common **multi-label combinations**.  
- Created a simplified target (`y_vis`) with four categories:
- `Top1` → most frequent single-label class  
- `Top2` → second most frequent single-label class  
- `MultiCombo` → most frequent multi-label pattern  
- `Other` → remaining samples  

This transformation allowed clear, color-coded plots without overplotting noise from 14 original labels.

### 3️⃣ Feature Scaling
- Applied **StandardScaler** to standardize all 103 features.  
- Explained why scaling is crucial for distance-based algorithms like *t-SNE* and *Isomap*.

---

## 🔍 t-SNE: Local Structure Visualization

### Implementation
- Used **sklearn.manifold.TSNE** to reduce data to 2D.  
- Tuned **perplexity** values (5, 30, 50) and evaluated stability of clusters.  
- Final choice: `perplexity = 30` (balanced local-global preservation).  

### Visualization
- Generated scatter plots with points colored by simplified label categories.  
- Observed:
- Local clusters of single-label samples.
- Dense overlaps showing label ambiguity.
- Sparse isolated points representing outliers.

### Insights
- **Noisy Labels:** Color mixing inside clusters.  
- **Outliers:** Distant isolated samples, possibly due to extreme expression patterns.  
- **Hard-to-learn samples:** Transition zones between clusters where colors overlap heavily.  

---

## 🌐 Isomap: Global Structure Visualization

### Implementation
- Used **sklearn.manifold.Isomap(n_neighbors=10, n_components=2)**.  
- Built a neighborhood graph and reconstructed global geodesic distances.  

### Visualization
- 2D projection colored by same categorical labels.  
- Revealed smooth manifold curvature — unlike t-SNE’s scattered local clusters.  

### Comparison to t-SNE
| Method | Preserves | Strength | Weakness |
|--------|------------|-----------|-----------|
| **t-SNE** | Local neighborhoods | Highlights label overlap, fine clusters | May distort global shape |
| **Isomap** | Global manifold geometry | Shows overall structure, curvature | Less effective for local noise |

---

## 📈 Manifold Complexity & Structure Preservation

Included metrics for quantitative assessment:
- **Trustworthiness Score** (from sklearn) — measures local structure retention.  
- **Isomap Residual Variance** — estimates reconstruction fidelity.  
- **Spearman Correlation (Pairwise Distances)** — correlation between original and reduced-space distances.

Visualizations and metrics confirm:
- t-SNE better preserves **local proximity**.
- Isomap better captures **global curvature** of the data manifold.

---

## 🧠 Key Observations

1. The Yeast dataset exhibits **high manifold curvature**, explaining multi-label complexity.  
2. **Label noise and overlap** are visible across both methods, affecting classification boundaries.  
3. t-SNE and Isomap complement each other — one reveals **local clusters**, the other **global geometry**.  
4. Manifold analysis acts as a **diagnostic tool** for dataset veracity before model training.

---

## 🧩 Technologies Used

| Library | Purpose |
|----------|----------|
| `numpy`, `pandas` | Data manipulation |
| `scikit-learn` | t-SNE, Isomap, metrics |
| `matplotlib`, `seaborn` | Visualization |
| `scipy` | Distance metrics, residual variance |
| `liac-arff`, `openml` | Data import |

---
