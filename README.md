# Acelerometro - Microbiota Study

Investigation of the relationship between moderate-to-vigorous physical activity (MVPA) and gut microbiota composition.

## 📋 Description

This project investigates the association between different levels of moderate-to-vigorous physical activity (MVPA) and gut microbiota composition in participants, utilizing 16S rRNA gene sequencing and advanced bioinformatic techniques.

## 🎯 Objectives

- Categorize participants into MVPA tertiles
- Characterize the diversity and composition of gut microbiota
- Identify differentially abundant taxa between MVPA groups
- Explore correlations between physical activity and microbiological parameters

---

## 📖 Methods

### Definition of Groups

**MVPA Tertile Classification.** Participants were categorized into tertiles based on total weekly moderate-to-vigorous physical activity (TotalMVPA) measured in minutes per week. Tertile cutpoints were calculated using the quantile() function with probabilities at 0%, 33.33%, 66.67%, and 100%, resulting in:

- **T1 (Low MVPA):** 6.77-372.94 min/week
- **T2 (Medium MVPA):** 372.95-570.34 min/week
- **T3 (High MVPA):** 570.35-1684.67 min/week

The cut() function with include.lowest=TRUE was used to assign samples to tertiles.

---

## 🔬 Measurements

### Gut Microbiota Measurements

**Sample Collection and Storage.** Participants provided and collected stool samples with the OMNIgene GUT (OMR 200) kit (DNAgenoteck, Ottawa, ON, Canada), which allows for storing stabilized DNA at room temperature for 60 days. The Cancer Research Center in Salamanca processed specimens, and DNA quality and amount were measured using techniques including:

- TapeStation 4200 (Agilent, Santa Clara, CA, USA)
- Qubit 4.0 fluorometer
- Nanodrop (Thermo Fisher Scientific, Waltham, MA, USA)

**Amplicon and Illumina Sequencing of Bacterial 16S rRNA Genes.** The V5–V6 region of the 16S rRNA gene was amplified using the following primers:


**Illumina adapter overhang nucleotide sequences:**
- Forward overhang: `5′ TCGTCGGCAGCGTCAGATGTGTATAAGAGACAG [locus-specific sequence]`
- Reverse overhang: `5′ GTCTCGTGGGCTCGGAGATGTGTATA AGAGACAG-[locus-specific sequence]`

To analyze the microbial communities, we amplified the V5–V6 regions of the 16S rRNA gene with a primer set, purified the resulting amplicons, and added indexes in a second PCR reaction. Each sample was designated using a unique combination of two indexes (S5XX-N7XX). We pooled the final amplicon libraries equimolarly and sequenced them on the Illumina MiSeq platform (Illumina, Inc. San Diego, CA, USA) using a v3 (600-cycle) reagent kit.

**Data Processing:** Raw sequence data were processed using a custom pipeline and quality passing-filter readings were grouped into operational taxonomic units (OTUs). As part of our quality control measures:
- Paired ends with overlap < 200 nt were discarded
- Chimeric sequences were removed using de novo chimera detection in USEARCH (https://www.drive5.com/usearch/)

---

## 📊 Statistical Analysis

### General Statistical Analysis

**Data Collection and Processing.** Data were collected using REDCap (Research Electronic Data Capture). Normality was assessed using the Shapiro-Wilk test. Continuous variables were presented as mean ± SD or median (IQR) for normally and non-normally distributed data, respectively; categorical variables were expressed as frequencies and percentages.

**Comparisons across MVPA tertiles:**
- Kruskal-Wallis tests for continuous variables
- Pairwise Wilcoxon tests with Bonferroni correction for post-hoc comparisons
- χ² tests for categorical variables

### Microbiome Bioinformatic Analysis

**Data Processing and Taxonomic Analysis.** Microbiome sequence data were processed and analyzed using RStudio (version 2025.05.1) with the mia package (version 1.17.5) for microbiome data analysis.

**Quality Control and Data Assessment:**
- Initial dataset: 472 taxa across 1,008 samples
- Average: 97 taxa detected per sample
- Data sparsity assessed by calculating the proportion of zero counts in the abundance matrix

**Data Filtering.** A data-driven filtering strategy was applied using the 5th percentile of empirical distributions as thresholds:
- Samples with library sizes < 6,133 reads: excluded
- Taxa with total counts < 8: excluded
- **Result:** 957 of 1,008 samples (94.9%) and 449 of 472 taxa (95.1%) retained

**Prevalence-based subsets:**
- **Core microbiome:** taxa present in ≥80% of samples
- **Common taxa:** ≥50% prevalence
- **Extended subset:** ≥20% prevalence

### Diversity Analysis

**Alpha Diversity.** Alpha diversity (observed richness, Shannon index) was compared across MVPA tertiles using:
- Kruskal-Wallis tests
- Pairwise Wilcoxon tests with FDR correction (Benjamini-Hochberg method)

**Beta Diversity.** Assessed using Aitchison distances on CLR-transformed data (pseudocount=1):
- Visualization via PCoA
- PERMANOVA (999 permutations) with betadisper to verify homogeneity of dispersions

### Correlation Analysis

**Spearman Rank Correlations.** Spearman rank correlations between genus-level relative abundances and clinical/demographic variables were calculated on CLR-transformed and standardized (z-scored) abundance data. FDR correction for multiple testing was applied using the Benjamini-Hochberg method.

### Differential Abundance Analysis

**ANCOM-BC (Analysis of Compositions of Microbiomes with Bias Correction)** was the primary method, which:
- Accounts for sampling fraction variability
- Applies bias correction to address the compositional nature of microbiome data
- Detects structural zeros
- Uses linear regression on log-transformed abundances with bias-corrected estimators

**Three analytical strategies:**
1. Unadjusted models comparing MVPA tertiles
2. Models adjusted for gender and age
3. Fully adjusted models including gender, age, smoking status, Mediterranean diet adherence, alcohol consumption (categorical), BMI category, waist circumference, and daily step count

**Complementary methods for robustness:**
- **edgeR:** likelihood ratio tests on multiple coefficients after TMM normalization and GLM fitting with negative binomial distributions
- **DESeq2:** likelihood ratio tests comparing full vs reduced models with "poscounts" size factor estimation
- **limma-voom:** F-tests after voom transformation and empirical Bayes moderation
- **ALDEx2:** Kruskal-Wallis tests on CLR-transformed data with Monte Carlo sampling (128 iterations)

Taxa detected by ≥3 methods were considered as robust findings.

### Statistical Considerations

- Statistical significance: **p < 0.05** or **FDR/adjusted p-value < 0.05**
- Additional effect size thresholds: **|LFC| > 0.5** for heatmap visualizations
- All statistical tests were **two-sided**
- MVPA tertiles defined with cutpoints at 33rd and 67th percentiles

### Software and Packages

| Package | Version | Function |
|---------|---------|----------|
| R | 4.5.1 | Statistical analysis |
| RStudio | 2025.05.1 | Integrated Development Environment |
| mia | 1.17.5 | Microbiome data handling |
| miaViz | 1.17.9 | Microbiome visualization |
| scater | 1.37.0 | Quality control metrics |
| edgeR | 4.7.3 | Differential abundance |
| DESeq2 | 1.49.3 | Differential abundance |
| limma | 3.65.3 | Differential abundance |
| ALDEx2 | 1.41.0 | Differential abundance |
| ANCOMBC | 2.11.1 | Differential abundance (primary) |
| vegan | 2.7.1 | PERMANOVA and betadisper |
| ComplexHeatmap | 2.25.2 | Heatmap visualization |
| shadowtext | 0.1.5 | Heatmap visualization |
| ggplot2 | 3.5.2 | Graphics |
| ggpubr | 0.6.1 | Figure composition |
| patchwork | 1.3.1 | Figure composition |
| dplyr | 1.1.4 | Data manipulation |
| tidyr | 1.3.1 | Data manipulation |
| haven | 2.5.5 | SPSS file import |
| openxlsx | 4.2.8 | Results export |
| officer | 0.7.0 | Vector graphics |
| rvg | 0.3.5 | Vector graphics |

---

## 📧 Contact

For more information about this study, please contact:
- **Investigator:** Rita Salvado ritasalvado@usal.es
- **GitHub:** [@ritasalvado-source](https://github.com/ritasalvado-source)

## 📄 License

This project is licensed under an appropriate license. See the LICENSE file for details.
