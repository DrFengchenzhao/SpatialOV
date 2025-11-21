# STARLETS - Spatiotemporal Tumor Clone Tracking System

STARLETS is a comprehensive computational framework for inferring clonal evolution and spatial dynamics in tissue samples from multi-omics data. This repository contains end-to-end codes to reproduce this pipeline with a small dataset (OV006 right ovary) from Spatial Tumor Evolution Panorama of Ovarian Cancer (Feng et al, 2025). You can also find our originally describing STARLETS published in XXX.

Please note that, we performed analysis using all ST slices from each person, thus results generated using this small dataset might be slightly different from what we did in our article.

## ⚙️ Environment

### R Environment
We tested with:
- R==4.3.3
- SPATA2==2.0.4
- SPATAwrappers==0.0.0.9000
- Seurat==4.4.0

For more details, please see the output of `sessionInfo()` in `01.clone_inference/1.1.spata2_infercnv.ipynb`.

### Python Environment
We successfully ran with:
- python==3.9.16
- scanpy==1.9.3
- squidpy==1.3.0
- numpy==1.25.2
- scipy==1.11.4
- pandas==1.3.5
- scikit-learn==1.2.1
- scvelo==0.2.5

Other versions of scanpy and squidpy should also work.

## Repository Structure
STARLETS/
├── 01.clone_inference/ # CNV analysis and Clone identification for ST data 
│ ├── 1.1.spata2_infercnv.ipynb # CNV inference using SPATA2 R package
│ ├── 1.2.cnv_cluster.ipynb # Clustering of CNV profiles and manullay assign malignant clones
│ ├── 1.3.spatial_cnv_prob.ipynb # Posterior probability calculation of non-tumor and tumor clones
│ ├── 1.4.plot_spatial_posterior.ipynb # Visualization of posterior probabilities
│ └── gene_pos.txt # Gene position annotations for CNV inference in 1.1.spata2_infercnv.ipynb
│
├── 02.spatial_clonal_transition/ # Spatial clonal dynamics analysis
│ ├── 02.1.calculate_coherence_score.ipynb # Spatial coherence quantification
│ ├── 02.2.define_structure.ipynb # Spatial clonal transition region definition
│ └── 02.3.correlation.ipynb # Mixed linear model for correlation analysis between clonal transition regions and spatial features

Data should be download from Zenodo.

## Input and output from STARLETS

1.1.spata2_infercnv.ipynb receives a SCE data structure containing ST count, metadata and coordinates from both reference (OV000_FT_1 in our example) and target ST slice (OV006_Ov_R_1 in our example). The outputs are CSV files for window-smoothed CNV matrix, metadata matrix and coordinates. When running InferCNV, window_length could be set according to the data. In our attempts, window_length=101, 151, 201 give consistent results.

1.2.cnv_cluster.ipynb reads CNV matrix from 1.1 and performs principal component analysis, leiden clustering and UMAP visualizations. Then clones are manually assigned according to WGS or scRNA-seq results. The output is h5ad file for assigned clones. Users might adjust leiden resolution to check clones related to WGS/scRNA-seq.

1.3.spatial_cnv_prob.ipynb and 1.4.plot_spatial_posterior.ipynb perform sensitivity analysis regarding clone assignments. First, we calculated centroids for each clone and posterior probability for each ST spots for each clonotype (1.3.spatial_cnv_prob.ipynb). Then, users might check posterior probabilities using defined tumor/stromal regions or others (1.4.plot_spatial_posterior.ipynb). 

02.1.calculate_coherence_score.ipynb calculates coherence scores in different neighborhood windows (n = 5, 10, 20 or 100, which are related to sequencing platform). Then coherence score is defined using observed and expected values.

02.2.define_structure.ipynb defines stable/intermediate and unstable clonal structures using a consensus voting strategy in a smoothing window n = 4. This generates spatial clone transition structures.

02.3.correlation.ipynb performs regressions for features of interests between clonal transition regions.

## Citation
If you feel that the STARLETS pipeline is useful for your study, please cite us:



