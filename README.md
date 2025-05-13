# Final Project: Single-cell RNA-seq Analysis of Hepatoblastoma Tumors

## Overview

This project analyzes single-cell RNA-seq (scRNA-seq) data from hepatoblastoma (HB) tumors, background liver, and patient-derived xenograft (PDX) samples based on the dataset and findings from the paper:

> Bondoc et al., 2021. "Identification of distinct tumor cell populations and key genetic mechanisms through single cell sequencing in hepatoblastoma" ([Nature Communications Biology](https://www.nature.com/articles/s42003-021-02562-8))

Using the Seurat R package, this analysis explores tumor heterogeneity, identifies cell populations, performs marker gene analysis and annotation, and replicates key findings from the original study.

---

## Dataset

* **Source**: Paper 1 (Bondoc et al., 2021)
* **Samples**: 7 total (3 HB tumors, 2 background livers, 2 PDXs)
* **Cells**: \~67,000 nuclei across all samples
* **Technology**: 10X Genomics v3, aligned to GRCh38

---

## Preprocessing and Quality Control

* **QC Metrics**: Genes per cell, UMIs per cell, % mitochondrial reads
* **Doublet Detection**: Performed using DoubletFinder
* **Filtering Thresholds**:

  * nFeature\_RNA > 500
  * nCount\_RNA > 800
  * % mitochondrial < 10%
* **Output**: Cells and genes retained before/after filtering

---

## Normalization and Feature Selection

* **Normalization**: Log-normalization using Seurat's `NormalizeData()`
* **Feature Selection**: `FindVariableFeatures(method = "vst")`
* **Highly Variable Genes**: Top 2,000 features retained

---

## Dimensionality Reduction and Clustering

* **PCA**: Performed on 2,000 HVGs
* **PCs Used**: Determined via elbow plot and variance explained
* **Clustering**: Louvain algorithm with resolution 0.5–1.0
* **Visualization**: UMAP of clusters and sample origin

---

## Integration

* Harmony used for batch correction across tumor, liver, and PDX samples
* Pre- and post-integration UMAPs shown

---

## Marker Gene Analysis

* Top 5 marker genes per cluster identified using `FindAllMarkers()`
* Table and heatmap of marker expression included

---

## Automatic Annotation

* **Tool**: SingleR (Aran et al. 2019)
* **Reference**: Human Primary Cell Atlas
* **Output**: Cluster annotations with visualization

---

## Manual Annotation

* Based on marker genes and literature
* Final annotations validated using multiple databases and papers
* Visuals include:

  * Heatmap of top markers per cluster
  * Violin plots for top 3 clusters
  * Annotated UMAP

---

## First Replicate: Cell Proportion Analysis

To evaluate compositional differences, we used cell-type annotations to quantify the proportion of each cell type across background liver, tumor, and PDX samples. A consistent enrichment of transformed tumor clusters (Tr2, Tr5) in tumor tissues was observed, with background liver showing enrichment for hepatocytes. Proportion shifts were visualized in a barplot. The trends agree with the original findings and support known biology of tumor expansion.

**Key tools**: Seurat annotations, dplyr, ggplot2
**Relevant citation**: Lin et al., 2022, Nat Biotechnol. [https://doi.org/10.1038/s41587-021-01005-0](https://doi.org/10.1038/s41587-021-01005-0)

---

## Second Replicate: Cell Signaling Analysis

We reconstructed a simplified cell-cell communication network using **CellChat**. The tool predicts signaling pathways and ligand-receptor interactions between cell types. Tumor subclusters (e.g., Tr2) showed outgoing signals to endothelial cells and immune cells, consistent with a supportive tumor microenvironment. WNT, FGF, and CXCL networks were particularly enriched.

**Key tool**: CellChat (Jin et al., 2021)
**Relevant citation**: Jin et al., 2021, Nat Commun. [https://doi.org/10.1038/s41467-021-21246-9](https://doi.org/10.1038/s41467-021-21246-9)

---

## Personal Analysis: Cell Cycle Phase Analysis

To assess proliferative potential of tumor cells, we computed cell cycle phase scores (S and G2M) using canonical markers. A large proportion of Tr2 and Tr5 cells were cycling, especially in PDX. This supports their hypothesized role in tumor initiation and progression.

**Tool**: Seurat’s CellCycleScoring()
**Relevant citation**: Tirosh et al., 2016, Science. [https://doi.org/10.1126/science.aad0501](https://doi.org/10.1126/science.aad0501)

---

## Environment

* **Tool**: R (Seurat v4)
* **Session Info**: Included at end of report

---

## Citation

Bondoc A, Glaser K, Jin K, et al. (2021). Identification of distinct tumor cell populations and key genetic mechanisms through single cell sequencing in hepatoblastoma. *Communications Biology*, 4:1049. [https://doi.org/10.1038/s42003-021-02562-8](https://doi.org/10.1038/s42003-021-02562-8)
