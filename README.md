# Pediatric Glioma MAPK Inhibitor Response Analysis

## Abstract

Acquired resistance to MAPK-targeted therapy is associated with an immunosuppressive, therapy-induced tumor secretome in adult melanoma (Obenauf lab). Here, I asked whether a similar transcriptional response occurs in a pediatric brain tumor model treated with the same class of 
drug. Using publicly available RNA-seq data (GEO: GSE249718) from a BRAF V600E-mutant pediatric low-grade glioma cell line (BT-40) treated with the BRAF inhibitor dabrafenib, I performed differential expression analysis (DESeq2) comparing untreated controls to cells at 6 hours of treatment. PCA confirmed clear separation between conditions (98% of variance on PC1). Four myeloid/microglia-recruiting chemokines — CX3CL1, CCL2, CCL7, and CXCL10 — were significantly and substantially 
upregulated upon treatment, reproducing the central finding of the original study (Kocher et al., 2024) using an independent reanalysis. These results support the idea that MAPK inhibition triggers a secreted, myeloid-recruiting transcriptional program that is not specific to adult melanoma, and may represent a shared resistance-associated mechanism worth investigating across tumor types and patient ages.

## Data Source

Data from Kocher et al., *Journal of Neuro-Oncology* 2024, deposited in 
GEO under accession [GSE249718](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE249718).

For this analysis, I used 6 samples from the BT-40 (BRAF V600E-mutant pediatric low-grade glioma) cell line:
- **control**: untreated cells at t=0 (n=3)
- **t6h**: cells treated with dabrafenib for 6 hours (n=3)

## Methods

- Raw featureCounts-style output files were downloaded from GEO
- Column identity (raw counts vs. FPKM vs. TPM) was determined by comparing column sums against expected magnitudes, since the files were undocumented and unheadered
- Differential expression analysis was performed using **DESeq2** in R
- Genes with fewer than 10 total reads across samples were filtered out
- Ensembl gene IDs were mapped to gene symbols using `org.Hs.eg.db`
- PCA was used to assess sample clustering and quality control

## Results

- PCA showed clear separation between control and t6h samples along PC1 (98% of variance), with tight within-group clustering, supporting the reliability of the differential expression results.
- CX3CL1 was among the top 5 most significantly differentially expressed genes genome-wide (log2FC = 11.6, padj = 2.5e-171).
- All four myeloid/microglia-recruiting chemokines highlighted in the original study were significantly upregulated at 6 hours:

| Gene | log2FC | padj |
|------|--------|------|
| CX3CL1 | +11.65 | 2.5e-171 |
| CCL2 | +7.98 | 8.2e-92 |
| CCL7 | +6.65 | 4.3e-06 |
| CXCL10 | +2.82 | 6.8e-08 |

- FOS, a canonical MAPK-pathway activity marker, was slightly decreased at this timepoint (log2FC = -0.56), consistent with active BRAF inhibition rather than pathway reactivation — indicating this timepoint captures the onset of the secretory response during active treatment, not the rebound phase described later in the original study.

## Limitations

- Only one timepoint (6h) of six available timepoints was analyzed
- Small sample size (n=3 per group)
- This analysis captures the early treatment response, not the drug-withdrawal rebound phase that was the focus of the original paper's title
- Raw count column identity was inferred computationally rather than confirmed via official documentation, due to lack of a column header or data dictionary in the deposited files

## Motivation

This analysis was done as independent, self-directed practice ahead of a PhD application, to explore whether a resistance mechanism identified in adult melanoma (therapy-induced myeloid recruitment, Obenauf lab) generalizes to a pediatric brain tumor model treated with the same drug class.

## Files
- `analysis.R` — full analysis script
- `glioma_deseq2_results.csv` — full DESeq2 results table with gene symbols
- `pca_plot_glioma.png` — PCA plot of sample clustering
- `glioma_deseq2_results.csv` — full DESeq2 results table with gene symbols
- `pca_plot_glioma.png` — PCA plot of sample clustering
