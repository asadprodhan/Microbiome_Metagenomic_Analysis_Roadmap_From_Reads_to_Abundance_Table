<h1 align="center">Microbiome Metagenomic Analysis Roadmap: From Reads to Abundance Table</h1>

<h3 align="center">M. Asaduzzaman Prodhan<sup>*</sup> </h3>

<div align="center"><b> School of Biological Sciences, The University of Western Australia </b></div>

<div align="center"><b> 35 Stirling Highway, Perth, WA 6009, Australia. <sup>*</sup>Correspondence: prodhan82@gmail.com </b></div>

<br />

<p align="center">
  <a href="https://github.com/asadprodhan/Microbiome_Metagenomic_Analysis_Roadmap_From_Reads_to_Abundance_Table#GPL-3.0-1-ov-file"><img src="https://img.shields.io/badge/License-GPL%203.0-yellow.svg" alt="License GPL 3.0" style="display: inline-block;"></a>
  <a href="https://orcid.org/0000-0002-1320-3486"><img src="https://img.shields.io/badge/ORCID-green?style=flat-square&logo=ORCID&logoColor=white" alt="ORCID" style="display: inline-block;"></a>
</p>

<br />

---

## **CONTENT**

- [Quality Control of Raw FASTQ Reads](#quality-control-of-raw-fastq-reads)
- [Adapter & Quality Trimming](#adapter--quality-trimming)
- [Taxonomic Classification (Kraken2)](#taxonomic-classification-kraken2)
- [Standardising Kraken2 TSV Taxonomic Rank Column](#standardising-kraken2-tsv-taxonomic-rank-column)
- [File Renaming](#file-renaming)
- [Kraken-BIOM Conversion](#kraken-biom-conversion)
- [Pipeline Summary](#pipeline-summary)

<br />

---

## **Quality Control of Raw FASTQ Reads**

### **Why**

To evaluate sequencing quality, identify adapter contamination, and check for issues before trimming or classification.

### **Input**

- `*.fastq.gz` (raw Illumina PE150 reads)

### **What was done**

- Automated FastQC run on all raw reads.
- Inspected metrics including base quality, GC content, duplication levels, and overrepresented sequences.

### **Output**

- `*_fastqc.html` (visual QC reports)  
- `*_fastqc.zip` (data summaries)

<br />

---

## **Adapter & Quality Trimming**

### **Why**

To remove adapters, low-quality bases, and short fragments that negatively impact classification accuracy.

### **Input**

- `*_R1.fastq.gz`, `*_R2.fastq.gz` (raw paired-end reads)  
- `NexteraPE-PE.fa` (adapter sequences)

### **What was done**

- Performed paired-end trimming including:  
  - Adapter clipping  
  - Removing low-quality leading/trailing bases  
  - Sliding-window quality trimming  
  - Minimum-length filtering  

### **Output**

- `*_R1.trim.fastq.gz`, `*_R2.trim.fastq.gz`  
- `*_R1un.trim.fastq.gz`, `*_R2un.trim.fastq.gz`

<br />

---

## **Taxonomic Classification (Kraken2)**

### **Why**

To assign each read to a taxonomic node using a high-performance k-mer classifier with a customised Kraken2 reference database.

### **Input**

- `*_R1.trim.fastq.gz`, `*_R2.trim.fastq.gz`  
- Custom Kraken2 database (pre-built)

### **What was done**

- Classified each sample using all available HPC CPU cores.
- Generated:  
  - Per-read taxonomic assignments  
  - Sample-level taxonomic summary tables  

### **Output**

- `*_taxo.out` (per-read taxonomic assignments)  
- `*_taxo.tsv` (sample-level taxonomic reports)

<br />

---

## **Standardising Kraken2 TSV Taxonomic Rank Column**

### **Why**

Kraken2 may output non-standard pseudo-rank codes (e.g., `R1`, `R2`) in the taxonomic rank column.  
These are **not valid taxonomic ranks**, and **kraken-biom rejects them**.  
They must be replaced with acceptable rank letters.

### **Input**

- `*.tsv` (Kraken2 sample reports)

### **What was done**

- Replaced all non-standard pseudo-rank values with valid, recognised rank placeholders (e.g., K, D).
- Ensured all TSV files conform to kraken-biom requirements.

### **Output**

- `*_fixed.tsv` (standards-compliant TSV reports)

<br />

---

## **File Renaming**

### **Why**

To rename the Kraken2 output files according to sample names defined in metadata.

### **Input**

- `*.csv` mapping file (`old_name,new_name`)
- Original Kraken2 files

### **What was done**

- Applied metadata-based renaming for consistent sample identifiers across the workflow.

### **Output**

- Dataset with harmonised sample names  
- File contents unchanged

<br />

---

## **Kraken-BIOM Conversion**

### **Why**

Downstream tools such as phyloseq, QIIME2, and machine-learning workflows require a unified BIOM abundance table.

### **Input**

- Cleaned `*_fixed.tsv` sample reports

### **What was done**

- Combined all Kraken2 TSV reports into a single JSON BIOM format file.
- Included hierarchical taxonomic structure and per-sample counts.

### **Output**

- `*.biom` (JSON BIOM abundance table)

<br />

---

## **Pipeline Summary**

- QC completed  
- Trimming completed  
- Kraken2 classification completed  
- TSV taxonomic-rank normalisation completed  
- Metadata-based renaming completed  
- BIOM creation completed  

<br />

---
