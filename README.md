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

| Step                             | Tool                      | Purpose                                                                                                                                                                                 | Input                                    | Output                                                          |
| -------------------------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- | --------------------------------------------------------------- |
| **1. Data Download**             | **SRA Toolkit**           | Downloads raw sequencing data from the NCBI Sequence Read Archive (SRA) and converts `.sra` files into paired-end compressed FASTQ files.                                               | SRA Accession (DRR013000–DRR013007)      | `*_1.fastq.gz`, `*_2.fastq.gz`                                  |
| **2. Quality Assessment**        | **FastQC**                | Evaluates sequencing quality by assessing per-base quality scores, GC content, sequence duplication, adapter contamination, and overrepresented sequences.                              | FASTQ files                              | HTML & ZIP quality reports                                      |
| **3. Read Trimming**             | **Trimmomatic**           | Removes adapter sequences, trims low-quality bases, filters short reads, and generates high-quality paired-end reads for downstream analysis.                                           | Raw FASTQ files                          | Cleaned paired FASTQ files                                      |
| **4. Post-trimming QC**          | **FastQC**                | Performs quality assessment again after trimming to verify improvements in read quality and successful adapter removal.                                                                 | Trimmed FASTQ files                      | Updated QC reports                                              |
| **5. Reference Genome Indexing** | **BWA Index**             | Builds index files for the reference genome to enable efficient sequence alignment. This step is performed only once per reference genome.                                              | Reference genome (GRCh38)                | Indexed reference files (`.amb`, `.ann`, `.bwt`, `.pac`, `.sa`) |
| **6. Read Alignment**            | **BWA-MEM**               | Aligns high-quality sequencing reads to the human reference genome and generates alignment files.                                                                                       | Trimmed FASTQ + Indexed Reference Genome | SAM file                                                        |
| **7. BAM Processing**            | **SAMtools**              | Converts, sorts, and indexes alignment files, preparing them for variant calling and downstream analyses.                                                                               | SAM file                                 | Sorted BAM file                                                 |
| **8. Duplicate Removal**         | **Picard MarkDuplicates** | Identifies and removes PCR duplicates to reduce false-positive variant calls and improve variant calling accuracy.                                                                      | Sorted BAM file                          | Deduplicated BAM file & duplication metrics                     |
| **9. Variant Calling**           | **FreeBayes**             | Detects genomic variants, including SNPs and small insertions/deletions (Indels), by comparing aligned reads to the reference genome.                                                   | Deduplicated BAM files                   | VCF file                                                        |
| **10. Variant Filtering**        | **BCFtools**              | Filters variants based on sequencing depth, genotype, and quality metrics to retain high-confidence variants.                                                                           | Raw VCF file                             | Filtered VCF file                                               |
| **11. Custom Variant Filtering** | **AWK**                   | Applies sample-specific filtering criteria to identify variants unique to cancer samples while excluding variants observed in control samples.                                          | Filtered VCF                             | Candidate mutation list                                         |
| **12. Variant Summary**          | **BCFtools Query**        | Extracts selected variant information (chromosome, position, reference allele, alternate allele, genotype, read depth) into a tabular format for downstream analysis and visualization. | Filtered VCF                             | TSV/Text summary                                                |


# Key Learnings

Built an end-to-end NGS analysis pipeline.
Performed quality assessment and alignment of sequencing reads.
Processed BAM files using SAMtools.
Identified and filtered genomic variants.
Strengthened Linux and Bash scripting skills.
Gained practical experience in cancer genomics workflows.








