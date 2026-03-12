---
title: "Bulk RNA-Seq Series – Post 6: From BAM to Count Matrices with featureCounts & HTSeq"
author: "Badran Elshenawy"
date: 2025-03-26T10:10:00Z
categories:
  - "Bioinformatics"
  - "Genomics"
  - "RNA-Seq"
  - "Transcriptomics"
tags:
  - "Bulk RNA-Seq"
  - "Count Matrix"
  - "featureCounts"
  - "HTSeq"
  - "Read Quantification"
  - "Computational Biology"
  - "Data Science"
description: "A complete guide to generating count matrices in RNA-Seq using featureCounts and HTSeq. Learn the key differences, commands, options, and how to ensure accurate read quantification."
slug: "bulk-rna-seq-count-matrix"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/bulk_rna_seq_count_matrix/"
summary: "Generate RNA-Seq count matrices with featureCounts and HTSeq. Learn about annotation matching, strandedness, sorting, and best practices for accurate read quantification."
featured: true
rmd_hash: 95416a665f7a1147

---

# 🔬 Bulk RNA-Seq Series -- Post 6: From BAM to Count Matrices with featureCounts & HTSeq

## 🛠 Why Counting Reads Matters

After aligning your RNA-Seq reads to a reference genome, the next step is to **quantify how many reads map to each gene**. This generates the **count matrix** --- a crucial input for differential expression tools such as **DESeq2**, **edgeR**, or **limma-voom**.

The count matrix forms the core of RNA-Seq analysis: - 📊 Rows = Genes (features) - 📊 Columns = Samples - 📊 Values = Number of mapped reads per gene

Let's explore the two most common tools for generating count matrices: **featureCounts** and **HTSeq-count**.

------------------------------------------------------------------------

## 🔧 featureCounts: Fast & Versatile

Developed as part of the **Subread package**, `featureCounts` is designed for efficient processing of large BAM files.

### 🔹 Why use featureCounts?

✔️ Extremely **fast and memory-efficient**  
✔️ Supports **multi-threading** for speed (`-T`)  
✔️ Accepts **GTF and GFF annotations**  
✔️ Handles **gene-level and exon-level** counting  
✔️ Easy integration into large pipelines

### 📘 Example Command:

``` bash
featureCounts -T 8 -a genes.gtf -o counts.txt aligned.bam
```

### 🔍 Option Breakdown

| Option          | Meaning                                        |
|-----------------|------------------------------------------------|
| `-T 8`          | Use 8 processing threads                       |
| `-a genes.gtf`  | Input annotation file (GTF format)             |
| `-o counts.txt` | Output file for count matrix                   |
| `-s 1`          | Strand-specific mode (0 = unstranded, 1 = yes) |

✅ Outputs a tab-delimited file with **gene IDs and raw counts**.

------------------------------------------------------------------------

## 🐍 HTSeq-count: Pythonic & Reliable

`HTSeq-count` is part of the **HTSeq Python package** and offers simplicity and reproducibility for RNA-Seq quantification.

### 🔹 Why use HTSeq-count?

✔️ Excellent compatibility with **Ensembl annotations**  
✔️ Reliable defaults and informative error messages  
✔️ Ideal for scripting in **custom workflows**  
✔️ Handles strandedness and different sorting methods

### 📘 Example Command:

``` bash
htseq-count -f bam -r pos -s no -i gene_id aligned.bam genes.gtf > counts.txt
```

### 🔍 Option Breakdown

| Option       | Description                                          |
|--------------|------------------------------------------------------|
| `-f bam`     | Input format (BAM)                                   |
| `-r pos`     | Input sorted by position                             |
| `-s no`      | No strand specificity (`yes` or `reverse` if needed) |
| `-i gene_id` | Attribute in GTF to use as gene ID                   |

✅ Outputs a gene-wise count table to standard output.

------------------------------------------------------------------------

## 📊 Key Considerations Before Counting

To ensure accurate read counting:

-   ✅ BAM files should be **sorted by coordinate**
-   ✅ Annotations must match the genome version used during alignment
-   ✅ Select the correct **strand mode** (`-s`) based on your library prep
-   ✅ Inspect logs and output for **warnings or read assignment failures**

------------------------------------------------------------------------

## 🧮 What Does the Output Look Like?

Both tools produce a matrix like this:

| Gene ID    | Sample1 | Sample2 | Sample3 |
|------------|---------|---------|---------|
| ENSG000001 | 2031    | 1987    | 2203    |
| ENSG000002 | 412     | 398     | 441     |

This **raw count matrix** is the essential input for DESeq2 normalization, variance stabilization, and modeling steps.

------------------------------------------------------------------------

## 📌 Key Takeaways

✔️ `featureCounts` is optimized for **speed and scale**, ideal for large datasets  
✔️ `HTSeq-count` is great for **small-scale, scriptable analysis**  
✔️ Proper sorting, strandedness, and annotation matching are critical  
✔️ Always check count summaries for anomalies

📌 **Next up: Preprocessing Count Matrices for DESeq2! Stay tuned! 🚀**

👇 Which counting tool do you prefer --- featureCounts or HTSeq? Let's discuss!

#RNASeq #featureCounts #HTSeq #CountMatrix #Transcriptomics #Genomics #Bioinformatics #ComputationalBiology #OpenScience #DataScience

