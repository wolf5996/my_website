---
title: "Bulk RNA-Seq Series – Post 10: Capstone & Final Recap"
author: "Badran Elshenawy"
date: 2025-04-10T12:00:00Z
categories:
  - "Bioinformatics"
  - "Genomics"
  - "RNA-Seq"
  - "Transcriptomics"
tags:
  - "Bulk RNA-Seq"
  - "Pipeline"
  - "Snakemake"
  - "FASTQ"
  - "GTF"
  - "HTSeq"
  - "featureCounts"
  - "Reproducibility"
  - "Computational Biology"
description: "A capstone overview of the entire Bulk RNA-Seq pipeline — from raw reads to reproducible results. Includes practical commands and summaries for every step in the workflow."
slug: "bulk-rna-seq-capstone"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/bulk_rna_seq_capstone/"
summary: "Master the RNA-Seq pipeline with this capstone recap. Includes practical examples, file structure tips, and command-line snippets to help you implement each step efficiently."
featured: true
rmd_hash: 7feb33aef780d4ef

---

# 🔬 Bulk RNA-Seq Series -- Post 10: Capstone & Final Recap

## ⚡ The Full Journey from Raw Reads to Results

Missed a post? No worries --- here's your **all-in-one summary** of the Bulk RNA-Seq Series 📚💡 Each section includes a quick explanation **and** a practical code snippet or file structure to make it as actionable as possible.

------------------------------------------------------------------------

## 1️⃣ Introduction to Bulk RNA-Seq Analysis

🧪 Bulk RNA-Seq captures average gene expression across whole tissues or cell populations.

📌 Key lesson: sound **experimental design** is critical --- think replicates, conditions, RNA integrity.

``` text
Design considerations:
- 3+ biological replicates per condition
- RNA Integrity Number (RIN) > 8
- Balanced sequencing depth (~30M reads/sample)
```

------------------------------------------------------------------------

## 2️⃣ Understanding RNA-Seq Reads & FASTQ Files

📦 FASTQ files are the raw material of RNA-Seq. Each read has: - A sequence ID - The nucleotide sequence - A separator line - ASCII-encoded quality scores

``` bash
@SEQ_ID
GATTTGGGGTTCAAAGCAGTATCGATCAAAGGGTGCCCGATAG
+
!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>>
```

✅ Quality scores help us decide what needs to be trimmed before alignment.

------------------------------------------------------------------------

## 3️⃣ Quality Control with FastQC & MultiQC

🔍 We evaluated read quality, GC content, and adapter contamination.

``` bash
# Run FastQC on all FASTQ files
fastqc *.fastq

# Aggregate results with MultiQC
multiqc .
```

✅ Tip: Check per-base sequence quality and adapter content before proceeding.

------------------------------------------------------------------------

## 4️⃣ Read Trimming & Filtering with Trimmomatic

✂️ Removes low-quality bases and adapter sequences.

``` bash
trimmomatic PE sample_R1.fastq sample_R2.fastq \
  trimmed_R1.fastq unpaired_R1.fastq \
  trimmed_R2.fastq unpaired_R2.fastq \
  ILLUMINACLIP:adapters.fa:2:30:10 SLIDINGWINDOW:4:20 MINLEN:36
```

🧼 Cleaner reads improve mapping rates significantly.

------------------------------------------------------------------------

## 5️⃣ Read Alignment with STAR, HISAT2 & Minimap2

🧭 Align your reads to a reference genome.

``` bash
# STAR alignment
STAR --genomeDir ref_genome/ --readFilesIn trimmed_R1.fastq trimmed_R2.fastq --runThreadN 8 --outFileNamePrefix sample_

# HISAT2 (alternative short-read aligner)
hisat2 -x genome_index -1 trimmed_R1.fastq -2 trimmed_R2.fastq -S sample.sam

# Minimap2 (for long reads)
minimap2 -ax splice ref.fa sample.fastq > aligned.sam
```

🎯 Output: BAM or SAM files for downstream quantification.

------------------------------------------------------------------------

## 6️⃣ From BAM to Count Matrices with featureCounts & HTSeq

📊 Generate gene-level expression counts.

``` bash
# featureCounts
featureCounts -T 8 -a annotation.gtf -o counts.txt sample.bam

# HTSeq-count
htseq-count -f bam -r pos -s no -i gene_id sample.bam annotation.gtf > counts.txt
```

🎯 These counts feed into DESeq2, edgeR, or limma for downstream analysis.

------------------------------------------------------------------------

## 7️⃣ FASTQ vs. FASTA

🔍 Know your formats:

-   **FASTQ**: sequences **+** quality scores (used for RNA-Seq input)
-   **FASTA**: sequences only (used for reference genomes)

``` bash
# FASTQ sample
@SEQ_ID
ACGT...
+
!!''...

# FASTA sample
>chr1
ACGTACGTACGT...
```

✅ Use FASTQ for raw reads, FASTA for index building and annotation.

------------------------------------------------------------------------

## 8️⃣ Automating Pipelines with Snakemake

🛠️ Define your entire workflow in a `Snakefile`, and let Snakemake handle the logic.

``` python
rule trim:
  input: "samples/{sample}.fastq"
  output: "trimmed/{sample}_trimmed.fastq"
  shell: "trimmomatic SE {input} {output} SLIDINGWINDOW:4:20 MINLEN:36"

rule align:
  input: "trimmed/{sample}_trimmed.fastq"
  output: "aligned/{sample}.bam"
  shell: "hisat2 -x genome -U {input} | samtools view -Sb - > {output}"
```

📁 Reproducibility and automation made easy.

------------------------------------------------------------------------

## 9️⃣ Understanding GTF & GFF Files for Feature Annotation

📍 Used to tell quantification tools where genes, transcripts, and features exist.

``` text
Example GTF line:
chr1    ensembl gene    11869   14409   .   +   .   gene_id "ENSG00000223972"; gene_name "DDX11L1";
```

🦠 Custom annotations (like ERVs) can reveal hidden expression layers.

------------------------------------------------------------------------

## 🔟 Final Thoughts: Wrapping It All Together

🧩 The full RNA-Seq pipeline:

    FASTQ ➡️ FastQC ➡️ Trimmomatic ➡️ STAR/HISAT2 ➡️ featureCounts ➡️ DESeq2

📌 From QC to quantification, each step builds toward biological insight.

🎓 Whether you're prepping for a big project or revisiting old data, you now have the tools and clarity to navigate RNA-Seq like a pro.

------------------------------------------------------------------------

## 👇 Your Turn!

Have you built your own RNA-Seq pipeline?  
Have updated annotations or workflow automation revealed something new?

Drop a comment --- I'd love to hear your story! 💬

#RNASeq #Transcriptomics #Bioinformatics #ComputationalBiology #NGS #DataScience #GeneExpression #Snakemake #GTF #FASTQ #Annotation #FeatureCounts #HTSeq #Genomics #Reproducibility #ScienceWorkflow

