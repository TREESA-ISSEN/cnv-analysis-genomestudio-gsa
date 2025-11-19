# CNV Analysis Using Illumina Global Screening Array (GSA)

This document explains how to perform and interpret Copy Number Variation (CNV) analysis using **GenomeStudio** and **GSA genotyping arrays**, including:

- Role of Manifest Files
- Role of Cluster Files
- Interpretation of Log R Ratio (LRR)
- Interpretation of B Allele Frequency (BAF)
- Common CNV patterns and biological meaning
- Literature references relevant to GSA CNV workflows

---

## 1. What is a Manifest File?

A **Manifest File** contains probe and SNP annotation for the Illumina array.

### Purpose

- Maps SNP ID, chromosome, genomic position
- Defines A/B alleles and probe sequences
- Required for genotype calling and CNV analysis
- Ensures GenomeStudio knows how to interpret raw array signals

### Where to get it

✔ Illumina downloads  
✔ Provided during array purchase  
✔ Format: `.bpm`, `.csv`, or `.txt`

---

## 2. What is a Cluster File?

A **Cluster File (.egt)** contains predefined genotype centroids for AA, AB, BB calls.

### Purpose

- Standardizes genotype calling across batches
- Reduces sample-specific shifts in clustering
- Improves consistency in multi-batch or low-quality samples

| Feature | Manifest File | Cluster File |
|---------|---------------|--------------|
| Defines | SNP location & annotation | Expected genotype clusters |
| Needed for | CNV + genotyping | Genotyping only |

➡ Note: **Cluster file is NOT required for CNV-only intensity analysis**

---

## 3. GenTrain Score

A quality metric (0–1) measuring cluster separation.

| GenTrain Score | Interpretation |
|----------------|----------------|
| ≥ 0.7 | High quality |
| 0.4–0.7 | Review required |
| ≤ 0.3 | Poor—exclude SNP |

Poor GenTrain SNPs degrade LRR/BAF reliability.

---

## 4. CNV Detection in GenomeStudio

CNV calling relies on two intensity-based metrics:

### 4.1 Log R Ratio (LRR)

Measures total signal intensity → detects copy number change.

| LRR Value | CN State | Interpretation |
|-----------|----------|----------------|
| ~0.00 | CN = 2 | Normal |
| -0.3 to -0.8 | CN = 1 | Heterozygous deletion |
| ≤ -1.0 | CN = 0 | Homozygous deletion |
| +0.2 to +0.5 | CN = 3 | Duplication |
| > +0.6 | CN ≥ 4 | Amplification |

---

### 4.2 B Allele Frequency (BAF)

Fraction of B-allele intensity.

| BAF Pattern | Interpretation |
|-------------|----------------|
| 0.0, 0.5, 1.0 | Normal diploid |
| Only 0.0 + 1.0 | Loss of heterozygosity / deletion |
| 0.0, 0.33, 0.66, 1.0 | Duplication (CN=3) |
| Random scattered 0–1 | Mosaicism or UPD |

---

## 5. Interpretation Workflow

✔ Check **LRR** → Does it shift up/down?  
✔ Check **BAF** → Are normal clusters preserved?  
✔ Compare region to:

- DGV
- ClinVar
- DECIPHER
- Internal CNV database

✔ Apply ACMG-CNV criteria (e.g., ClinGen technical standards)

---

## 6. Example CNV Interpretation

| Pattern | LRR | BAF | Interpretation |
|---------|-----|-----|----------------|
| Flat at 0.0 | 0.0 | 0,0.5,1 | Normal |
| Negative shift | -0.5 | 0 & 1 only | Heterozygous deletion |
| Positive shift | +0.35 | 0,0.33,0.66,1 | Duplication |

---

## 7. When NOT to Use Genotype Cluster File

❌ CNV calling does NOT require genotype calls  
✔ **"Intensity Only" mode** can be used  
✔ Genotype failures do NOT stop CNV analysis

---

## 8. Key Literature

**Highly recommended papers for interpretation workflow:**

1️⃣ **The individual and global impact of copy-number variants on complex human traits**  
📎 https://doi.org/10.1016/j.ajhg.2022.03.013

2️⃣ **Development and validation of a pharmacogenomics reporting workflow based on the Illumina GSA array**  
📎 https://doi.org/10.3389/fphar.2024.1349203

---

## 9. Notes From Practical Use

✔ GSA reliably detects CNVs > 50–100 kb  
✔ Mosaic CNVs require manual inspection  
✔ Always verify large CNVs with:

- IGV (BAM-based depth)
- MLPA or qPCR (clinical validation)
- SNP density matters (weak signal in telomeric / centromeric regions)

---

## 10. Maintainer

**Treesa K. Issen**  
Rare Disease Genomics | CNV + Exome Interpretation  
CSIR-IGIB, Delhi

