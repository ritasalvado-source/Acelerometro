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
