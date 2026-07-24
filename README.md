An R-based collection of reproducible workflows for cancer transcriptomic analysis, differential-expression testing, biomarker prioritisation, functional enrichment, and biological interpretation.

The repository currently includes workflows for:

- GEO microarray acquisition and analysis
- Integration of multiple lung-cancer microarray datasets
- Quality control, normalisation, and batch-effect correction
- TCGA-STAD bulk RNA-seq acquisition and preprocessing
- Differential-expression analysis with `limma`, `edgeR`, and `TCGAbiolinks`
- PCA, correlation heatmaps, volcano plots, and DEG heatmaps
- Gene Ontology and KEGG pathway enrichment
- Gene Set Enrichment Analysis
- Gene–concept and transcription-factor network visualisation
- Export of candidate genes for downstream tools

> **Scope note:** The current repository contains microarray and bulk RNA-seq workflows. A complete single-cell analysis workflow is not yet included.

## Repository overview

| File or directory | Description |
|---|---|
| [`R codes of Transcriptome analysis pipeline of lung cancer .R`](R%20codes%20of%20Transcriptome%20analysis%20pipeline%20of%20lung%20cancer%20.R) | Integration and analysis of the GSE19804 and GSE31210 lung-cancer microarray datasets |
| [`MicroarrayAnalysis_ShayanJL.R`](MicroarrayAnalysis_ShayanJL.R) | GEO microarray download, preprocessing, differential-expression analysis, PCA, heatmap, and volcano-plot workflow |
| [`TCGA_Data_Analysis_Using_TCGAbiolinks_and_DEA_ShayanJL.R`](TCGA_Data_Analysis_Using_TCGAbiolinks_and_DEA_ShayanJL.R) | TCGA-STAD RNA-seq download, preprocessing, gene annotation, sample selection, and differential-expression analysis |
| [`Functional_Enrichment_Gene_Ratio_Analysis_ShayanJL.R`](Functional_Enrichment_Gene_Ratio_Analysis_ShayanJL.R) | Calculation and visualisation of functional-enrichment gene ratios and adjusted p-value categories |
| [`The plots and the results/`](The%20plots%20and%20the%20results/) | Selected plots, DEG tables, enrichment results, and network visualisations |

## Analysis workflows

### 1. Integrated lung-cancer microarray analysis

The main lung-cancer workflow integrates the GEO datasets `GSE19804` and `GSE31210`.

The workflow includes:

1. Reading and merging Affymetrix CEL files
2. Importing sample and batch metadata
3. Array-level quality assessment
4. RMA background correction and normalisation
5. Batch-effect correction with `ComBat`
6. Comparison of raw, preprocessed, and batch-corrected data
7. PCA, correlation heatmaps, and boxplots
8. Probe-to-gene-symbol annotation
9. Removal of unmapped and duplicated probes
10. Expression-based gene filtering
11. Differential-expression analysis with `limma`
12. Volcano-plot and top-DEG heatmap generation
13. Gene Ontology enrichment
14. KEGG pathway enrichment
15. Gene Set Enrichment Analysis
16. Gene–concept network visualisation
17. Transcription-factor enrichment analysis
18. Candidate-gene export for downstream tools

### 2. GEO microarray analysis

The GEO workflow uses `GEOquery` to retrieve `GSE106817` and includes:

- Sample-group selection
- Expression-matrix extraction
- Log transformation
- Quantile normalisation
- Sample-distribution boxplots
- Correlation heatmaps
- Principal component analysis
- Differential-expression analysis with `limma`
- FDR-adjusted DEG selection
- Volcano-plot generation
- Top-DEG heatmap generation

The sample labels and comparison groups are analysis-specific and should be verified before reusing the script for another dataset.

### 3. TCGA-STAD bulk RNA-seq analysis

The TCGA workflow uses `TCGAbiolinks` to query and download harmonised TCGA stomach adenocarcinoma data.

The workflow includes:

- GDC project discovery
- TCGA-STAD RNA-seq querying
- Data download and preparation
- Preprocessing of available assay formats
- Ensembl-to-HUGO gene-symbol conversion
- Selection of protein-coding genes
- Identification of primary tumour, normal-tissue, and metastatic samples
- Differential-expression analysis with `edgeR`
- Differential-expression analysis with `limma`

The workflow uses harmonised `hg38` data and should be reviewed against the current `TCGAbiolinks` data structure before execution.

### 4. Functional-enrichment gene-ratio visualisation

The enrichment workflow:

1. Reads a cleaned enrichment table from Excel
2. Calculates gene ratio as:

   `Overlap_Count / Overlap_Total`

3. Orders enriched terms by gene ratio
4. Divides adjusted p-values into quartile-based categories
5. Produces a publication-ready horizontal bar chart
6. Exports the figure at 300 DPI

## Selected outputs

The [`The plots and the results/`](The%20plots%20and%20the%20results/) directory includes examples such as:

- PCA comparisons of raw, preprocessed, and batch-corrected data
- Expression-distribution boxplots
- Sample-correlation heatmaps
- Volcano plots
- Top-DEG heatmaps
- Gene Ontology results
- KEGG pathway results
- GSEA plots
- Gene–concept networks
- Transcription-factor networks
- Differentially expressed gene tables
- Gene lists for external downstream tools

## Requirements

R version 4.x or a compatible recent release is recommended.

Major packages used across the workflows include:

### Data access and preprocessing

- `GEOquery`
- `Biobase`
- `affy`
- `simpleaffy`
- `affyPLM`
- `arrayQualityMetrics`
- `affyQCReport`
- `TCGAbiolinks`
- `SummarizedExperiment`
- `EDASeq`
- `sva`

### Statistical analysis

- `limma`
- `edgeR`
- `WGCNA`

### Annotation and enrichment

- `AnnotationDbi`
- `hgu133plus2.db`
- `org.Hs.eg.db`
- `clusterProfiler`
- `enrichplot`
- `msigdbr`

### Data handling and visualisation

- `dplyr`
- `tidyr`
- `readxl`
- `ggplot2`
- `pheatmap`
- `EnhancedVolcano`
- `forcats`
- `scales`

## Installation

Clone the repository:

```bash
git clone https://github.com/shayanjl/Genomic_Analysis_and_Biomarker_Discovery.git
cd Genomic_Analysis_and_Biomarker_Discovery
```

Install `BiocManager` if it is not already available:

```r
if (!requireNamespace("BiocManager", quietly = TRUE)) {
  install.packages("BiocManager")
}
```

Install the required CRAN and Bioconductor packages according to the workflow you plan to run.

Example:

```r
install.packages(c(
  "dplyr",
  "tidyr",
  "readxl",
  "ggplot2",
  "pheatmap",
  "forcats",
  "scales"
))

BiocManager::install(c(
  "GEOquery",
  "Biobase",
  "affy",
  "affyPLM",
  "simpleaffy",
  "arrayQualityMetrics",
  "affyQCReport",
  "sva",
  "limma",
  "edgeR",
  "WGCNA",
  "EnhancedVolcano",
  "TCGAbiolinks",
  "SummarizedExperiment",
  "EDASeq",
  "AnnotationDbi",
  "hgu133plus2.db",
  "org.Hs.eg.db",
  "clusterProfiler",
  "enrichplot"
))
```

## Usage

Before running a workflow:

1. Clone or download the repository.
2. Open the required R script.
3. Replace placeholder working directories such as `D:/XlocationX/` or `E:/XlocationX/`.
4. Create the required `Data`, `Results`, and `QC` directories.
5. Download or provide the required input files.
6. Check sample labels and metadata carefully.
7. Review all analysis thresholds before execution.
8. Run the script section by section in RStudio.

Example:

```r
source("MicroarrayAnalysis_ShayanJL.R")
```

For the lung-cancer workflow:

```r
source("R codes of Transcriptome analysis pipeline of lung cancer .R")
```

## Expected lung-cancer input structure

The integrated lung-cancer workflow expects a structure similar to:

```text
Data/
├── GSE19804/
│   └── Affymetrix CEL files
├── GSE31210/
│   └── Affymetrix CEL files
└── Combined Meta Lung Cancer.csv

Results/
└── Generated tables and figures

QC/
└── Quality-control reports
```

The metadata file should contain sample identifiers and columns describing batch, tissue class, and analysis group.

Verify that the metadata row order matches the expression-data column order before analysis.

## Reproducibility notes

These scripts are research workflows and analysis templates rather than a fully automated software package.

Before reuse:

- Replace local path placeholders.
- Verify the required input files.
- Check directory-name capitalisation, including `Results` versus `results`.
- Review sample labels and group definitions.
- Confirm that metadata and expression matrices are aligned.
- Review outlier-removal decisions.
- Reassess log-fold-change and FDR thresholds.
- Check current package documentation for changed functions or data structures.
- Record package versions with `sessionInfo()`.

A reproducible analysis should save:

- Input-data accession numbers
- Metadata and group definitions
- Filtering criteria
- Outlier decisions
- Differential-expression thresholds
- Package versions
- Random seeds where applicable
- Final processed data and result tables

## Important limitations

- Input data are not fully bundled with the repository.
- Some scripts require manual path and metadata configuration.
- Analysis thresholds are dataset-specific and should not be reused automatically.
- Selected outliers are removed explicitly in the lung-cancer workflow and should be independently justified before replication.
- Package interfaces and public database structures may change over time.
- A complete single-cell workflow is not currently included.
- The workflows are intended for research and educational use, not clinical diagnosis or treatment decisions.

## Data sources

The workflows use publicly available data from:

- [NCBI Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/)
- [The Cancer Genome Atlas](https://www.cancer.gov/ccg/research/genome-sequencing/tcga)
- [NCI Genomic Data Commons](https://portal.gdc.cancer.gov/)

Users are responsible for following the terms and citation requirements of the original datasets.

## Author

**Shayan Jalali**

- GitHub: [github.com/shayanjl](https://github.com/shayanjl)
- Portfolio: [shayanjl.github.io](https://shayanjl.github.io/)
- ORCID: [0009-0005-3909-2920](https://orcid.org/0009-0005-3909-2920)

## Disclaimer

This repository is provided for research, education, and reproducibility. It does not constitute medical advice and has not been validated for clinical decision-making.
