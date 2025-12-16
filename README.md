Project 1: SARS-CoV-2 Infection Dynamics in Human Bronchial Epithelial Cells
📌 Overview

This repository contains a single-cell RNA-sequencing (scRNA-seq) analysis pipeline reproducing key results from:

Ravindra et al., 2021
“Single-cell longitudinal analysis of SARS-CoV-2 infection in human bronchial epithelial cells”
PLOS Biology
https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3001143

The analysis investigates cell-type composition, infection dynamics, and differentiation trajectories of SARS-CoV-2-infected human bronchial epithelial cells (HBECs) across multiple time points.

📊 Data Description
Source

GEO accession: GSE166766

Link: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE166766

Experimental Design

Human bronchial epithelial cells cultured at air–liquid interface were infected with SARS-CoV-2 and profiled using scRNA-seq at:

Mock (uninfected)

1 day post-infection (1 dpi)

2 days post-infection (2 dpi)

3 days post-infection (3 dpi)

Each sample is provided as a 10x Genomics matrix (matrix.mtx, barcodes.tsv, features.tsv).

🧪 Analysis Pipeline

The full pipeline is implemented in a single Jupyter Notebook (.ipynb) using Scanpy.

1. Data Acquisition

Downloaded raw 10x matrices from GEO

Organized files per sample (mock, 1 dpi, 2 dpi, 3 dpi)

2. Quality Control & Preprocessing

For each sample independently:

Filtered genes detected in fewer than 10 cells

Filtered cells with:

<200 detected genes

20% mitochondrial reads

Normalized total counts

Log-transformed expression values

Selected highly variable genes (HVGs)

3. Per-Sample Clustering (Exploratory)

PCA on HVGs

k-nearest neighbor graph

UMAP embedding

Leiden clustering

This step verifies sample quality and structure before joint analysis.

4. Joint Neighborhood Clustering (Paper Reproduction)

To match the reference paper:

All samples were concatenated into a single AnnData object

HVGs recomputed across all stages

PCA → neighbors → UMAP → Leiden clustering performed jointly

Enables direct comparison across infection stages

5. Cell Type Identification

Clusters were annotated using canonical airway epithelial markers:

Cell Type	Key Markers
Basal	KRT5, TP63
Club	SCGB1A1
Ciliated	FOXJ1, TPPP3
(Minor types visualized)	ENO2, CHGA, CHGB, POU2F3

Marker expression was visualized on UMAPs and clusters were assigned cell types based on dominant marker scores.

6. ACE2 and ENO2 Analysis

Visualized ACE2 and ENO2 expression on joint UMAPs

Computed:

Mean ACE2 expression per cluster

Percentage of ACE2-positive cells

Focused analysis on 3 dpi to assess infection relevance

7. Trajectory & Pseudotime Analysis

Diffusion Map and Diffusion Pseudotime (DPT)

Basal cells used as the root population

Revealed a differentiation trajectory:

Basal → Club → Ciliated

This mirrors epithelial differentiation and infection-associated remodeling reported in the paper.

❓ Questions & Answers (Project 1)
1. What cell types were identified at different stages of infection?

Three major epithelial cell types were identified:

Basal cells

Club cells

Ciliated cells

Their relative distributions shift across infection stages, with increased differentiation toward ciliated states at later time points.

2. Why do these cell types correlate with COVID-19 infection?

SARS-CoV-2 primarily targets airway epithelial cells. Infection perturbs:

Differentiation programs

Secretory and regenerative responses

Cell-state transitions rather than creating new cell types

Thus, infection correlates with changes in epithelial state composition, not simply cell presence or absence.

3. Is ACE2 a good marker for tracking COVID-19 infection rate in this dataset?

No.

ACE2 expression is:

Extremely low

Diffuse across clusters

Not visually enriched at 3 dpi

Therefore, ACE2 transcript abundance is not a reliable proxy for infection rate in this scRNA-seq dataset.

4. What is the difference between ENO2 and ACE2 as biomarkers?

ACE2: Viral entry receptor; indicates susceptibility but poorly detected here.

ENO2: Cell-identity marker highlighting a small neuroendocrine-like population.

ENO2 shows localized expression, whereas ACE2 does not define clusters.

5. Which cluster has the highest ACE2 expression at 3 dpi and what does it mean biologically?

Leiden cluster 5 has the highest mean ACE2 expression at 3 dpi.

However, expression levels are very low (<10% positive cells).

Interpretation:
ACE2 expression is biologically weak and does not define infected populations. Infection dynamics are better explained by cellular responses and differentiation trajectories.
