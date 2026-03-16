# PageRank on the arXiv High Energy Physics Collaboration Network

**A Distributed Link Analysis Study using Apache Spark**

**Author:** Selam Mahmud Ali
**Course:** Algorithms for Massive Data — MSc in Data Science for Economics
**Supervisor:** Prof. Dario Malchiodi — A.Y. 2025/2026

---
This is the link:

https://colab.research.google.com/drive/1HH187zaOejIBwTjkZXWkcCKWb0hHy11E?usp=sharing

---

## Overview

This project implements distributed weighted PageRank on the arXiv High
Energy Physics co-authorship network using Apache Spark. The goal is to
identify the most structurally central authors across three HEP sub-fields:

- `hep-th` — Theory
- `hep-ph` — Phenomenology
- `hep-ex` — Experiment

The central finding is that authors who bridge all three sub-fields
consistently achieve the highest PageRank scores, regardless of execution
environment or sample size.

---

## How to Run on Google Colab

1. Click the **Open in Colab** badge above
2. Add your Kaggle credentials to Colab Secrets:
   - Click the 🔑 key icon in the left sidebar
   - Add `KAGGLE_USERNAME` → your Kaggle username
   - Add `KAGGLE_KEY` → your Kaggle API key
   - Toggle **Notebook access ON** for both
3. Run all cells in order from top to bottom

> The notebook will install all dependencies and download the dataset
> (~4 GB). The first run takes 30–60 minutes due to the download and
> the PageRank convergence loop.

---

## How to Run Locally


The local notebook is available in `LinkAnalysisProject_JNotebook.ipynb`.

1. Install dependencies:
```bash
pip install pyspark findspark kaggle networkx matplotlib
```

2. Set Kaggle credentials:
```bash
export KAGGLE_USERNAME=your_username
export KAGGLE_KEY=your_api_key
```

3. Open the notebook and run all cells in order.

To reproduce the scaling analysis at different sample fractions (10%, 15%,
20%, 25%), change the `sample_fraction` variable in the data loading cell:
```python
sample_fraction = 0.10  # change to 0.15, 0.20, or 0.25 as needed
```

Note that higher sample fractions require more memory and longer runtime.


---

## Software Requirements

| Dependency | Version |
|------------|---------|
| Python     | 3.10+   |
| PySpark    | 3.x / 4.1.1 |
| NetworkX   | 3.x     |
| matplotlib | 3.x     |
| findspark  | 2.x     |
| kaggle     | 1.x     |
| JDK        | 17      |

---

## Dataset

Cornell arXiv metadata snapshot, available on Kaggle:
https://www.kaggle.com/datasets/Cornell-University/arxiv

- Format: newline-delimited JSON (~4 GB)
- Sample fraction: **5%** (`seed = 42`)
- Filter: only `hep-th`, `hep-ph`, `hep-ex` papers retained

---

## Key Results

| Environment | Sample | Nodes | Top Author | Iterations |
|-------------|--------|-------|------------|------------|
| Google Colab | 5% | ~43,000 | Y. Zhang | 37 |
| Local (Apple Silicon) | 5% | 7,444 | John Ellis | 42 |
| Local | 10% | — | Y. Li | — |
| Local | 15% | — | Y. Zhang | — |
| Local | 20% | — | Y. Zhang | — |
| Local | 25% | — | Y. Zhang | — |

Y. Zhang consistently dominates at larger sample fractions and in the
denser Colab sample, confirming this is a genuine structural property
of the network rather than a sampling artefact.

---

## Note on Reproducibility

Results differ between local (Apple Silicon) and Google Colab (x86) due
to hardware-dependent differences in Spark's random number generator.
Despite using `seed = 42` and `repartition(16)`, the two environments
draw different subsets of the 4 GB file. Both environments confirm the
core finding: full pipeline bridge authors achieve the highest PageRank.
See Section 7.5 of the report for a full discussion.

---

## Report

The full project report is in `report/Algorithms_Project_Final.pdf`.
