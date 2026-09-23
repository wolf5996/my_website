<div align="center">

# 🧬 Bioinformatics & Computational Biology Blog

**Tutorial series for biologists who want to run their own analysis**

![Posts](https://img.shields.io/badge/posts-114-success)
![Series](https://img.shields.io/badge/series-13-8A2BE2)
![Since](https://img.shields.io/badge/since-December%202024-lightgrey)
![R](https://img.shields.io/badge/R-tidyverse%20%7C%20Bioconductor-276DC3?logo=r&logoColor=white)
![Python](https://img.shields.io/badge/Python-scverse-3776AB?logo=python&logoColor=white)

**Read it at [badran-elshenawy.netlify.app](https://badran-elshenawy.netlify.app/)**

</div>

---

## 📌 At a Glance

| | |
|---|---|
| **What** | Tutorial series on single-cell, spatial and bulk RNA-seq, R, Python, and the tools around them |
| **Who it's for** | Biologists who are one clear explanation away from running their own analysis |
| **Approach** | What a tool actually does, where its assumptions hide, and how to use it on real data |
| **Scale** | **114 posts** across 13 series |
| **Author** | [Badran Elshenawy](https://www.linkedin.com/in/dr-badran-m-e-65414b113/), computational biologist at the University of Oxford |

---

## 🗺️ How the Series Fit Together

The analysis series build on each other; the tooling series sit alongside and can be read in any order.

```mermaid
flowchart LR
    A["📊 Tidyverse<br/>R foundations"] --> B["🧬 Foundational Genomics<br/>Bioconductor data structures"]
    B --> C["🧪 Bulk RNA-Seq<br/>end-to-end pipeline"]
    C --> D["📈 DESeq2<br/>differential expression"]
    D --> E["🔬 Single-Cell RNA-Seq<br/>Seurat, QC, clustering"]
    E --> F["🌿 Trajectories<br/>pseudotime, RNA velocity"]
    E --> G["🖱️ The Loupe Ecosystem<br/>10x single-cell and spatial"]
    A --> H["🐍 Tidyverse to Scverse<br/>moving to Python"]

    T1["🔁 Git"] ~~~ T2["🔒 Lockfiles"]
    T2 ~~~ T3["⌨️ Command Line<br/>+ The Modern Terminal"]
    T3 ~~~ T4["🤖 Claude Code"]

    classDef analysis fill:#e8f1fb,stroke:#276DC3,color:#0b2545
    classDef tooling fill:#f4f0fb,stroke:#8A2BE2,color:#2d0a4e
    class A,B,C,D,E,F,G,H analysis
    class T1,T2,T3,T4 tooling
```

---

## 📚 The Series

### 🧬 Analysis

| Series | Posts | What it covers | Start here |
|---|---:|---|---|
| **Foundational Genomics** | 12 | Biostrings, GenomicRanges, Rle vectors, genome arithmetic, annotation | [Rle vectors](https://badran-elshenawy.netlify.app/posts/genomic-rle-vectors/) |
| **Bulk RNA-Seq** | 10 | An end-to-end bulk RNA-seq pipeline | [Introduction](https://badran-elshenawy.netlify.app/posts/bulk-rna-seq-introduction/) |
| **DESeq2** | 17 | Differential expression from raw counts to results | [Introduction](https://badran-elshenawy.netlify.app/posts/rnaseq-analysis-introduction/) |
| **Single-Cell RNA-Seq** | 15 | Seurat, QC, clustering, differential expression, pathway analysis | [Practice datasets](https://badran-elshenawy.netlify.app/posts/scrna-seq-seuratdata-practice-datasets/) |
| **Trajectories** | 7 | Pseudotime, Monocle3, RNA velocity, PAGA, choosing a method | [The roadmap](https://badran-elshenawy.netlify.app/posts/advanced-scrnaseq-trajectory-intro/) |
| **The Loupe Ecosystem** | 3 | 10x Loupe Browser, `.cloupe` files and `loupeR` | [Point-and-click EDA](https://badran-elshenawy.netlify.app/posts/loupe-ecosystem-post-1-point-and-click-eda/) |

### 💻 Languages

| Series | Posts | What it covers | Start here |
|---|---:|---|---|
| **Tidyverse** | 14 | Data wrangling, ggplot2, purrr, functional programming in R | [Why the tidyverse](https://badran-elshenawy.netlify.app/posts/tidyverse-intro/) |
| **Tidyverse to Scverse** | 7 | Moving single-cell work from R to Python: uv, Quarto, marimo, Polars | [Why I'm switching](https://badran-elshenawy.netlify.app/posts/tidyverse-to-scverse-why-switching/) |

### 🛠️ Tools and Reproducibility

| Series | Posts | What it covers | Start here |
|---|---:|---|---|
| **Git** | 8 | Version control for researchers | [Why you need Git](https://badran-elshenawy.netlify.app/posts/git-version-control-part1/) |
| **Lockfiles** | 2 | Reproducible environments with `uv.lock` and `renv.lock` | [The missing layer](https://badran-elshenawy.netlify.app/posts/lockfiles-reproducibility-layer/) |
| **Command Line** | 3 | Terminal essentials for bioinformaticians | [dust and lsd](https://badran-elshenawy.netlify.app/posts/cli-tools-dust-lsd/) |
| **The Modern Terminal** | 4 | Modern replacements for the core CLI tools: `eza`, `zoxide`, `dust`, `ouch` | [eza](https://badran-elshenawy.netlify.app/posts/modern-terminal-post-1-eza/) |
| **Claude Code** | 9 | AI-assisted research: skills, MCPs, subagents, plugins | [Skills](https://badran-elshenawy.netlify.app/posts/claude-skills-turn-claude-into-your-specialist/) |

Plus 3 standalone posts, on VS Code and Quarto visual mode, the tidyverse transition, and where it all started.

---

## 🧭 Where to Start

| If you want to... | Read |
|---|---|
| 🔬 Analyse your first single-cell dataset | **Single-Cell RNA-Seq**, then **Trajectories** |
| 📈 Find differentially expressed genes in bulk data | **Bulk RNA-Seq**, then **DESeq2** |
| 📊 Get comfortable in R first | **Tidyverse**, then **Foundational Genomics** |
| 🐍 Move your analysis to Python | **Tidyverse to Scverse** |
| 🖱️ Explore 10x data without code | **The Loupe Ecosystem** |
| 🔁 Make your analysis reproducible | **Git**, then **Lockfiles** |
| ⌨️ Work faster on a cluster | **Command Line**, then **The Modern Terminal** |
| 🤖 Use AI in your research workflow | **Claude Code** |

---

## 🖼️ What the Posts Look Like

### A workflow shift

Each series post carries one infographic that has to make its point without the surrounding text.

<p align="center">
  <img src="content/posts/images/tidyverse_to_scverse_scanpy_done_right_pipeline.png" width="85%" alt="The 2019 Python stack versus Polars, Plotnine, marimo and uv">
</p>

### A tool, old versus new

The command-line series compare the classic tool with its modern replacement, one tool per post.

<p align="center">
  <img src="content/posts/images/modern_terminal_dust_one_tree.png" width="85%" alt="du, one level at a time, versus dust, one tree">
</p>

<details>
<summary><b>📊 More figures: lockfiles, the Loupe ecosystem, eza, zoxide, ouch</b></summary>

#### Builds without and with a lockfile
<img src="content/posts/images/lockfiles_without_vs_with_lockfile.png" alt="Builds without and with a lockfile">

#### loupeR turns a Seurat object into a shareable .cloupe
<img src="content/posts/images/loupe_ecosystem_louper_seurat_bridge.png" alt="loupeR turns a Seurat object into a shareable .cloupe">

#### The same directory listed by ls and by eza
<img src="content/posts/images/modern_terminal_eza_vs_ls.png" alt="The same directory listed by ls and by eza">

#### cd one level at a time versus zoxide jumping by frecency
<img src="content/posts/images/modern_terminal_zoxide_frecency_jumping.png" alt="cd one level at a time versus zoxide jumping by frecency">

#### One tool and one syntax for every archive format
<img src="content/posts/images/modern_terminal_ouch_unified_archives.png" alt="One tool and one syntax for every archive format">

</details>

---

## 📄 License

Blog content © Badran Elshenawy. The Hugo Coder theme is licensed under the [MIT License](https://github.com/luizdepra/hugo-coder/blob/main/LICENSE.md).

---

<div align="center">

**Badran Elshenawy** · [Pathania Group](https://www.pathanialab.com/team.html) · Ludwig Institute for Cancer Research, University of Oxford

</div>
