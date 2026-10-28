TCGA-STAD: tumor vs. normal differential expression and pathway enrichment

Differential expression (DESeq2), gene set enrichment analysis (GSEA) and GO/KEGG enrichment on TCGA stomach adenocarcinoma RNA-seq, following the analytical workflow of Figure 7 in Jiang et al., Cell Reports Medicine 2023.

Repository structure
.
├── README.md                       this report
├── TCGA_STAD_DEG_GSEA.ipynb        full code, outputs and interpretation
└── figures/                        all plots and result tables
    ├── 01_volcano.png
    ├── 02_gsea_go_NES.png
    ├── 03_gsea_kegg_bubble.png
    ├── 04_enrichment_top_GO.png
    ├── 05_enrichment_top_KEGG.png
    ├── 06a_ora_go_up.png
    ├── 06b_ora_go_down.png
    ├── 07a_ora_kegg_up.png
    ├── 07b_ora_kegg_down.png
    ├── 08_squamous_marker_score.png
    ├── 09_pca.png
    ├── 09b_pca_squamous.png
    ├── 10_pvalue_histogram.png
    ├── 11_ma_plot.png
    ├── 12_dispersion.png
    ├── 13_top_gene_counts.png
    ├── deseq2_results.csv
    ├── gsea_go_results.csv
    ├── gsea_kegg_results.csv
    ├── ora_go_up.csv
    ├── ora_go_down.csv
    ├── ora_kegg_up.csv
    ├── ora_kegg_down.csv
    ├── positive_controls.csv
    └── squamous_marker_score.csv
Output files

All figures and tables are in figures/.

Result tables (CSV)

File	Contents
figures/deseq2_results.csv	All tested genes: Ensembl ID, gene symbol, Entrez ID, base mean, log2FC, shrunken log2FC, Wald statistic, p-value, adjusted p-value, DEG status
figures/gsea_go_results.csv	Significant GO Biological Process terms from GSEA, with NES, adjusted p-value and leading-edge genes
figures/gsea_kegg_results.csv	Significant KEGG pathways from GSEA
figures/ora_go_up.csv	GO over-representation, genes up-regulated in tumors
figures/ora_go_down.csv	GO over-representation, genes down-regulated in tumors
figures/ora_kegg_up.csv	KEGG over-representation, genes up-regulated in tumors
figures/ora_kegg_down.csv	KEGG over-representation, genes down-regulated in tumors
figures/positive_controls.csv	Positive-control genes: expected vs. observed direction
figures/squamous_marker_score.csv	Squamous marker score for each sample

Figures (PNG)

File	Contents
figures/01_volcano.png	Volcano plot, tumor vs. normal
figures/02_gsea_go_NES.png	GSEA: top GO terms by NES
figures/03_gsea_kegg_bubble.png	GSEA: top KEGG pathways
figures/04_enrichment_top_GO.png	Enrichment plot, top GO term
figures/05_enrichment_top_KEGG.png	Enrichment plot, top KEGG pathway
figures/06a_ora_go_up.png	GO over-representation, up in tumor
figures/06b_ora_go_down.png	GO over-representation, down in tumor
figures/07a_ora_kegg_up.png	KEGG over-representation, up in tumor
figures/07b_ora_kegg_down.png	KEGG over-representation, down in tumor
figures/08_squamous_marker_score.png	Squamous marker score by group
figures/09_pca.png	PCA of variance-stabilized counts
figures/09b_pca_squamous.png	PCA colored by squamous marker score
figures/10_pvalue_histogram.png	Raw p-value distribution
figures/11_ma_plot.png	MA plot (shrunken log2FC)
figures/12_dispersion.png	DESeq2 dispersion estimates
figures/13_top_gene_counts.png	Normalized counts for the most significant gene (COL10A1)
Design

Comparison. Figure 7 of Jiang et al. correlates gene expression with the deep learning radiomics score (DLRS), which requires running the HR-Rad-Net model on matched CT images. Because that is not feasible in a standalone demonstration, primary tumor vs. solid tissue normal was used as a stand-in to apply the same analytical steps.

Data. TCGA-STAD STAR gene counts from GDC (TCGAbiolinks): all 36 solid tissue normal samples and a random subset of 60 of 412 primary tumors (seed = 42).

Methods.

DESeq2 on raw counts (genes with ≥ 10 reads in ≥ 10 samples), Wald test, tumor vs. normal; apeglm shrinkage for fold-change thresholds and plotting.
DEGs: Benjamini-Hochberg adjusted p < 0.05 and |shrunken log2FC| > 1.
Pre-ranked GSEA (clusterProfiler) on GO Biological Process and KEGG, ranked by the DESeq2 Wald statistic.
Over-representation analysis of up- and down-regulated DEGs separately, against a background of all tested genes.
Quality control: PCA, p-value distribution, MA and dispersion plots, positive-control genes, leading-edge genes, and a squamous marker score to assess sample composition.
Results
Summary

Differential expression analysis of 60 primary gastric adenocarcinomas and 36 normal samples identified 6,855 DEGs. Relative to normal gastric tissue, tumors showed coordinated up-regulation of DNA replication, cell cycle and DNA repair programs and of extracellular matrix genes, and coordinated down-regulation of mitochondrial oxidative metabolism, fatty acid beta-oxidation and xenobiotic metabolism.

Quality control

PCA of variance-stabilized counts separated tumor and normal samples along PC1 (37% of variance). The raw p-value distribution, MA plot and dispersion estimates were consistent with expected model behavior. All seven positive-control genes changed in the expected direction:

Gene	Expected	Shrunken log2FC	Adjusted p
MKI67	Up	1.84	3.3e-12
TOP2A	Up	2.53	1.6e-19
GKN1	Down	-2.54	7.4e-04
GKN2	Down	-2.85	1.2e-04
ATP4A	Down	-2.35	7.0e-16
ATP4B	Down	-2.79	4.5e-04
PGC	Down	-0.80	0.11 (n.s.)

PCA

figures/09_pca.png

<details> <summary>Model diagnostics: p-value distribution, MA plot, dispersion estimates</summary>

p-value histogram

figures/10_pvalue_histogram.png

MA plot

figures/11_ma_plot.png

Dispersion estimates

figures/12_dispersion.png

</details>
Differentially expressed genes

COL10A1 was the most significant DEG (shrunken log2FC ≈ 7.3), with approximately 100-fold higher median normalized expression in tumors. Other highly ranked up-regulated genes included COL11A1, CTHRC1 and MMP11, consistent with extensive extracellular matrix remodeling in the tumor stroma.

Volcano plot

figures/01_volcano.png

<details> <summary>COL10A1 normalized counts by sample</summary>

COL10A1 counts

figures/13_top_gene_counts.png

</details>
Gene set enrichment analysis

Pre-ranked GSEA identified 1,251 GO Biological Process terms and 153 KEGG pathways at adjusted p < 0.05.

Up-regulated in tumors (NES > 0). Positively enriched gene sets were dominated by DNA replication, replication fork processing, cell cycle checkpoint signaling, homologous recombination and interstrand cross-link repair, with corresponding KEGG pathways including Cell cycle, Homologous recombination, the Fanconi anemia pathway and Ribosome biogenesis in eukaryotes. The leading edge of DNA-templated DNA replication comprised core replisome, origin-firing and fork-protection components, including RFC3, RFC4, PRIM2, MCM10, CDC7, DBF4 and TIMELESS. Over-representation analysis of up-regulated DEGs independently recovered mitotic processes (organelle fission, nuclear division, chromosome segregation). Together, these results indicate a highly proliferative tumor state with elevated DNA replication and repair activity.

Down-regulated in tumors (NES < 0). Negatively enriched gene sets were dominated by mitochondrial respiration (oxidative phosphorylation, respiratory electron transport chain, ATP synthesis coupled electron transport), fatty acid oxidation, and cytochrome P450-mediated xenobiotic and drug metabolism. The leading edge of fatty acid catabolic process included ACOX1, ECHS1, ETFA, PPARA and PPARD. Over-representation analysis of down-regulated DEGs identified muscle contraction, blood circulation and regulation of hormone levels, likely reflecting the smooth muscle, vascular and endocrine components of the normal gastric wall that are underrepresented in tumor samples.

Several KEGG terms require cautious interpretation. The Chemical carcinogenesis pathways consist largely of phase I and phase II detoxification enzymes, and disease-annotated pathways (Parkinson disease, Thermogenesis, Non-alcoholic fatty liver disease) share mitochondrial respiratory-chain genes with oxidative phosphorylation. Their enrichment reflects the same underlying signals rather than disease-specific biology.

GSEA GO

figures/02_gsea_go_NES.png

GSEA KEGG

figures/03_gsea_kegg_bubble.png

Enrichment plot: DNA-templated DNA replication

figures/04_enrichment_top_GO.png

<details> <summary>More enrichment results: top KEGG enrichment plot and over-representation analysis by direction</summary>

Enrichment plot: top KEGG pathway

figures/05_enrichment_top_KEGG.png

GO ORA up in tumor

figures/06a_ora_go_up.png

GO ORA down in tumor

figures/06b_ora_go_down.png

KEGG ORA up in tumor

figures/07a_ora_kegg_up.png

KEGG ORA down in tumor

figures/07b_ora_kegg_down.png

</details>
Comparison with Jiang et al. (2023), Figure 7

Jiang et al. compared DLRS-low with DLRS-high tumors, whereas this analysis compares tumor with normal tissue, so the two analyses address different biological contrasts.

Overlap. DNA repair, enriched in DLRS-low tumors (Fig. 7A), and DNA replication and mismatch repair, which appeared among their KEGG results (Fig. 7B), were among the most strongly up-regulated programs in tumors here.
Difference. Oxidative phosphorylation and fatty acid metabolism, enriched in DLRS-low tumors, were down-regulated in tumors relative to normal tissue. This is expected: tumor-normal comparisons are dominated by differences in tissue composition and differentiation, whereas the DLRS contrast captures heterogeneity among tumors.
Not expected here. The immune-suppressive signatures associated with DLRS-low tumors reflect intertumoral variation in the tumor microenvironment and would not be expected to emerge from a tumor-normal comparison.
Sample composition

PCA revealed heterogeneity among normal samples. Approximately 7 of 36 normal samples formed a distinct cluster on PC2 and showed high expression of squamous epithelial markers (KRT5, KRT13, KRT4, CSTA and MAL). Because gastric mucosa is a non-keratinizing glandular epithelium, these samples likely contain esophageal squamous mucosa obtained near the gastroesophageal junction, and they account for the keratinization and cornified envelope formation terms among the negatively enriched gene sets. A subset of tumors showed intermediate squamous scores, consistent with tumors located at or near the junction. A small number of normal samples clustered with tumors, which may reflect tumor-adjacent tissue alterations or tumor cell contamination.

PCA colored by squamous score

figures/09b_pca_squamous.png

<details> <summary>Squamous marker score by group</summary>

Squamous marker score

figures/08_squamous_marker_score.png

</details>
Limitations
The tumor-normal contrast is a surrogate for the DLRS comparison. It captures broad differences between malignant and non-malignant tissue, not the immune-infiltration axis reflected by DLRS.
A random subset of 60 of 412 primary tumors was analyzed, without adjustment for technical covariates such as tissue source site.
Bulk RNA-seq profiles reflect cell-type composition, so part of the observed differences likely arises from stromal, immune and epithelial content rather than tumor cell-intrinsic changes.
The squamous subgroup of normal samples contributes to a subset of negatively enriched terms.
Over-representation analysis on approximately 6,800 DEGs favors broad terms; GSEA was treated as the primary pathway evidence.
Future directions
Repeat the analysis excluding squamous-high normal samples to test the robustness of the main findings.
Analyze the full tumor cohort with tissue source site as a covariate.
Test MSigDB Hallmark gene sets for direct comparison with Figure 7A.
Stratify tumors by an immune-infiltration score (e.g., CD8A, GZMA and PRF1 expression) to approximate the biological axis captured by DLRS, and repeat GSEA.
Reproducing the analysis

Open TCGA_STAD_DEG_GSEA.ipynb in Google Colab with the R runtime and run all cells. The first run installs Bioconductor packages (about 35 minutes) and downloads about 400 MB from GDC. All figures and tables are written to figures/.

References
Jiang Y, et al. Non-invasive tumor microenvironment evaluation and treatment response prediction in gastric cancer using deep learning radiomics. Cell Reports Medicine 4, 101146 (2023).
Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology 15, 550 (2014).
Zhu A, Ibrahim JG, Love MI. Heavy-tailed prior distributions for sequence count data. Bioinformatics 35, 2084-2092 (2019).
Wu T, et al. clusterProfiler 4.0: a universal enrichment tool for interpreting omics data. The Innovation 2, 100141 (2021).
Subramanian A, et al. Gene set enrichment analysis. PNAS 102, 15545-15550 (2005).
Colaprico A, et al. TCGAbiolinks: an R/Bioconductor package for integrative analysis of TCGA data. Nucleic Acids Research 44, e71 (2016).
