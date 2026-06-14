# Cancer-vs-Control-NGS-Variant-Analysis
End-to-end NGS variant analysis workflow for comparative cancer genomics using publicly available sequencing datasets.

# Overview

This project demonstrates an end-to-end bioinformatics workflow for identifying and comparing genomic variants between cancer and non-cancer samples using publicly available dataset on SRA database.

The objective of this project is to perform variant analysis through a reproducible pipeline, beginning with raw sequencing reads and progressing through quality assessment, reference genome alignment, variant calling, filtering, and downstream visualization.

# Objectives
* Process raw NGS sequencing data from publicly available datasets.
* Perform quality assessment and preprocessing of sequencing reads.
* Align reads to the human reference genome.
* Identify genomic variants through variant calling.
* Filter variants based on quality metrics.
* Visualization using IGV

# Dataset

Source: NCBI Sequence Read Archive (SRA)

# Study: Mutation profiles in inflamed gastric epithelium with Helicobacter pylori infection during the development of gastric cancer.
# RUN
DRR013000
DRR013001
DRR013002
DRR013003
DRR013004
DRR013005
DRR013006
DRR013007

# Workflow 

Raw Data Acquisition (SRA Toolkit)
↓
Raw Reads (.fastq.gz or. sra)
↓
Quality Control Using Trimming (FastQC, Trimmomatic)
↓
Cleaned Reads
↓
Alignment (BWA, Bowtie2)
↓
Aligned Reads (. sam or. bam)
↓
Post-Alignment Processing (Samtools, Picard)
↓
Processed Aligned Reads
↓
Variant Calling (Freebayes)
↓
Unfiltered Variants (.vcf)
↓
Filtering 
↓
Annotation & Visualization (VEP, IGV)


