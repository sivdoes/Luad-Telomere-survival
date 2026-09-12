# Telomere Maintenance in Lung Adenocarcinoma: A Multi-Omics Survival Analysis

How do cancer cells get past the Hayflick limit? How do they achieve this pseudo immortality? They basically use one of two main ways:

1. **The Telomerase Pathway:** Reactivating the TERT protein and using the TERC RNA template to rebuild telomeres.
2. **Alternative Lengthening of Telomeres (ALT):** A recombination-based process dependent on the destruction of chromatin Proteins (ATRX/DAXX) and the hijacking of DNA repair mechanics (RAD52/PML).

This project looks at how these two pathways affect patient survival in Lung Adenocarcinoma (LUAD). I specifically tested if mRNA expression levels or actual DNA mutations are what really drive the clinical outcomes.

## Cohort & Methodology
* **Data Sources:** TCGA-LUAD dataset. Downloaded clinical phenotypes and bulk RNA-seq data from UCSC Xena, and got the targeted SNV/CNA mutation data from cBioPortal.
* **Cohort Size:** 525 complete-case patients (189 mortality events).
* **Multi-Omics Integration:** Matched RNA expression with DNA level mutation data for a 6 gene panel (TERT, TERC, ATRX, DAXX, PML, RAD52).
* **Statistical Modeling:** Stage-stratified Cox Proportional Hazards model.
* **Correction:** Identified a non-proportional hazards violation in TERT and corrected it using a time-varying interaction term.

## Key Findings

### 1. Telomerase Transcription is the Primary Driver of Early Mortality
TERT mRNA expression is a strong predictor of overall survival (HR = 1.41, p < 0.0005 at baseline). The hazard is time dependent, where there is an aggressive peak in the first year attenuating there after. Interestingly, just having a TERT DNA mutation alone (HR = 0.76, p = 0.258) didn't show significance. This shows the real danger is when the cell actively produces the telomerase mRNA, not just when it has a mutated gene sitting there.

### 2. The ALT Pathway is Dormant in LUAD
For the ALT pathway to work, tumors usually need to mutate ATRX or DAXX to open up the telomere. But in this LUAD cohort, ATRX/DAXX mutations were pretty rare (only 8%) and didn't have prognostic significance. Because of this, the downstream ALT genes (RAD52, PML) also showed no independent impact on survival. LUAD majorly favors the path of least resistance that is telomerase reactivation.

## Repository Structure
* `notebooks/03_luad_telomere_final_pipeline.ipynb`: The complete, reproducible pipeline from raw data import to the final time-varying Cox model.
* `data/`: (Ignored via `.gitignore`) Scripts to pull and format TCGA data.
* `results/`: Contains the output statistical tables and the time-varying hazard ratio curve.

## Future Directions
This analysis gives good evidence for TERT mRNA being a primary driver in LUAD, but future work should validate this in a completely different cohort. Also, doing actual physical telomere-length assays instead of just looking at genomic data would give a much more accurate classification of ALT vs. Telomerase tumors.
