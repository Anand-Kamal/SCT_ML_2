# Customer Segmentation — K-Means Clustering

Segmenting mall customers into behavioral groups using unsupervised
K-Means clustering on the classic
["Mall Customer Segmentation Data"](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python)
dataset.

## Overview

Unlike a prediction task, this dataset has no target column — there's no
known "right answer" to learn from. Instead, the goal is to discover
natural groupings of customers based on income and spending behavior, so a
business could target each group with different marketing strategies.

**Method:** K-Means clustering on `Annual Income` and `Spending Score`,
with the number of clusters chosen using the elbow method and silhouette
score rather than guessed.

**Result:** 5 clusters, silhouette score 0.55 — confirmed by both the
elbow plot and the silhouette score independently agreeing on `k=5`.

## Segments Found

| Cluster | Label | Count | Avg Age | Avg Income | Avg Spending Score |
|---|---|---|---|---|---|
| 1 | Target / VIP — high income, high spending | 39 | 32.7 | $86.5k | 82.1 |
| 3 | Careful Savers — high income, low spending | 35 | 41.1 | $88.2k | 17.1 |
| 2 | Young Spenders — low income, high spending | 22 | 25.3 | $25.7k | 79.4 |
| 4 | Budget-Conscious — low income, low spending | 23 | 45.2 | $26.3k | 20.9 |
| 0 | Standard — mid income, mid spending | 81 | 42.7 | $55.3k | 49.5 |

**How a business could use this:**
- **Target/VIP** — highest priority for retention and loyalty programs
- **Careful Savers** — has spending power but isn't using it here; the
  segment with the most upside if marketing can shift behavior
- **Young Spenders** — price-sensitive but engaged; good fit for
  promotions and discounts
- **Budget-Conscious** — lowest priority for marketing spend
- **Standard** — the largest, "everyone else" segment

## Methodology

1. **EDA** — distributions of age, income, and spending score; gender
   breakdown; a scatter plot of income vs. spending score that already
   shows visible clumping before any clustering is applied.
2. **Feature scaling** — `StandardScaler` on income and spending score.
   K-Means uses Euclidean distance, so features on different scales (income
   in thousands of dollars vs. a 1–100 score) need to be standardized first,
   or income would dominate the distance calculation purely because its
   numbers are larger.
3. **Choosing k** — inertia (elbow method) and silhouette score computed
   for k = 2 through 10; both point to k = 5.
4. **Fit K-Means** with k = 5, visualize clusters and centroids.
5. **Profile each cluster** — average age, income, and spending score per
   group, translated into descriptive, business-friendly labels.
6. **Export** a labeled CSV with each customer's assigned cluster.

## Repository Structure

```
.
├── mall_customer_segmentation.ipynb   # main notebook (Google Colab-ready)
├── Mall_Customers.csv                 # input data
├── mall_customers_segmented.csv       # output: customers with cluster labels
└── README.md
```

## Running the Notebook

**Option 1 — Google Colab (recommended)**
1. Open [colab.research.google.com](https://colab.research.google.com)
2. Upload `mall_customer_segmentation.ipynb`
3. Run all cells — the second cell will prompt you to upload
   `Mall_Customers.csv`

**Option 2 — Locally / Jupyter**
```bash
pip install pandas numpy matplotlib seaborn scikit-learn
jupyter notebook mall_customer_segmentation.ipynb
```

## Requirements

- Python 3.8+
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn

## Notes & Limitations

- Clustering used only `Annual Income` and `Spending Score`. `Age` and
  `Gender` were explored in EDA but left out of the clustering itself to
  keep segments interpretable on two axes; a natural extension is adding
  `Age` as a third clustering dimension.
- K-Means assumes roughly spherical, similarly-sized clusters. It fits this
  dataset well (silhouette score 0.55), but wouldn't be the right choice
  for data with very differently shaped or overlapping groups — density-based
  methods like DBSCAN would be a better fit there.
- There's no ground-truth label to validate against (this is unsupervised
  learning), so "accuracy" isn't a meaningful metric here — silhouette
  score and visual inspection of cluster separation are used instead.

## Acknowledgments

Dataset: ["Mall Customer Segmentation Data"](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python),
Kaggle.
