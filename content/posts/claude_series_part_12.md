---
title: "Claude Series – Post 12: I Built Custom Claude Code Skills for Bioinformatics - And Open-Sourced Them!"
author: "Badran Elshenawy"
date: 2026-03-16T09:00:00Z
categories:
  - "AI Tools"
  - "Bioinformatics"
  - "Claude"
  - "R Programming"
  - "Open Source"
tags:
  - "Claude Code"
  - "Skills"
  - "SKILL.md"
  - "scRNA-seq"
  - "Seurat"
  - "BadranSeq"
  - "Quarto"
  - "R Packages"
  - "Bioinformatics"
  - "Computational Biology"
  - "Reproducibility"
  - "AI Workflow"
description: "How I built five custom Claude Code Skills for my bioinformatics workflow — covering R coding, project scaffolding, Quarto notebooks, lab documentation, and R package development — and why I open-sourced them."
slug: "claude-skills-bioinformatics-open-source"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/claude_skills_bioinformatics_open_source/"
summary: "Post 12 in the Claude series. Moving from the concept of Skills (Post 6) to a real implementation — five opinionated, interconnected Skills for R, scRNA-seq, and scientific documentation, with BadranSeq baked in as the default visualization layer."
featured: true
rmd_hash: 0962cfc698959018

---

# From Concept to Practice: Building Claude Code Skills for Bioinformatics

Back in Post 6, I introduced Claude Code Skills --- the idea that you can package domain-specific instructions into reusable `SKILL.md` files that Claude automatically loads when relevant. That post was about the *what* and the *why*.

This post is about the *how*. Specifically, how I built five custom Skills that cover my entire bioinformatics research workflow --- and why I'm open-sourcing them for the community to fork and adapt.

The repository lives here: [github.com/wolf5996/claude-skills](https://github.com/wolf5996/claude-skills)

## WHY BUILD CUSTOM SKILLS? 🎯

If you've been following this series, you know the progression. CLAUDE.md (Post 5) gives Claude project-level context. Skills (Post 6) teach Claude *how* to do specific tasks. MCPs (Post 7) let Claude reach external tools. Subagents (Post 8) let Claude parallelise work.

But here's the thing --- all of that machinery is generic. It doesn't know that your lab uses a specific directory layout. It doesn't know that you prefer tidyverse over base R. It doesn't know that every code chunk in your Quarto notebooks should be self-contained and independently runnable.

Custom Skills are where generic AI tooling becomes *yours*. You're encoding the conventions, patterns, and quality standards that you've spent years developing --- and handing them to Claude as executable documentation.

The key insight: **Skills aren't prompts you type repeatedly. They're institutional knowledge that persists across every session.**

## THE FIVE SKILLS 🔬

Let me walk through each one and why it exists.

### 1. creating-analysis-projects 🧪

**The problem:** Every bioinformatics project starts the same way --- you need a directory structure that separates raw data from code from intermediate outputs from final results. And you need that separation to be *strict*, because raw data should never be git-tracked while your analysis scripts absolutely should be.

**What the Skill enforces:**

-   A `read/` → `scripts/` → `checkpoints/` → `write/` directory architecture
-   Raw data lives in `read/` and stays out of version control
-   Intermediate objects get saved as `.rds` checkpoints --- making every analysis step independently resumable
-   Final outputs land in `write/` for clean export

**Why it matters:** When Claude scaffolds a new project, it doesn't just create folders. It creates the *right* folders with the *right* `.gitignore` patterns and the *right* README templates. Every project starts clean and stays reproducible.

### 2. writing-r-code 💻

This is the most detailed Skill in the collection, and it reflects years of opinions about what good R code looks like in a bioinformatics context.

**Core conventions:**

-   **Tidyverse-first:** Pipes, `dplyr` verbs, `ggplot2` --- not base R equivalents
-   **Self-contained chunks:** Every code chunk follows a strict `Libraries → Inputs → Processing → Outputs` structure. Each chunk loads its own data from a checkpoint file rather than depending on in-memory state from previous chunks. This makes chunks independently runnable and debuggable
-   **Seurat v5 patterns:** Layer-based operations, SCTransform v2, the modern API --- not the Seurat v3 patterns that LLMs love to hallucinate

**The BadranSeq visualization layer:** This Skill enforces a strict visualization fallback chain: BadranSeq → SCpubr → Seurat. If you're not familiar with BadranSeq --- I built it ([github.com/wolf5996/BadranSeq](https://github.com/wolf5996/BadranSeq)) because I got tired of spending more time styling Seurat plots than doing actual biology.

The core problem is that Seurat's default plots aren't publication-ready. PCA plots don't show variance explained on the axes. Cells in dense clusters blend together with no borders. Split comparisons via faceting lose spatial context because you only see cells in each subset --- not where they sit relative to the full dataset.

BadranSeq fixes all of this with native ggplot2 wrappers:

-   `do_PcaPlot()` automatically calculates and displays variance explained percentages on axes --- no more manually computing `(stdev^2 / sum(stdev^2)) * 100`
-   `do_UmapPlot()` adds black cell borders for clarity and shows cluster labels by default
-   `do_FeaturePlot()` uses viridis color scales (perceptually uniform, colorblind-friendly) with expression cutoff support
-   Split comparisons render grey silhouettes of all cells as background context, so you never lose the spatial reference

The aesthetic approach is inspired by SCpubr, but BadranSeq is deliberately lightweight --- pure ggplot2, no heavy dependencies. If you need SCpubr's full feature set with dozens of specialized plot types, use SCpubr. If you want the core aesthetics with minimal overhead, BadranSeq is the answer.

By baking BadranSeq into the `writing-r-code` Skill as the first choice, every scRNA-seq figure Claude generates comes out publication-ready by default. No more post-hoc styling. No more "I'll make it pretty later." It's pretty from the first render.

**The Context7 integration:** This Skill mandates that Claude verifies function signatures against live documentation via the Context7 MCP for 17+ R and Bioconductor packages before writing code. No more deprecated function calls. No more hallucinated arguments. If Context7 says the function doesn't take that parameter, Claude doesn't write it.

This single convention alone has saved me more debugging time than everything else combined.

### 3. writing-qmd-scientific 📝

**The controversial rule:** All explanatory prose in Quarto notebooks must be bullet points. No paragraphs. Ever.

This sounds extreme, but there's a reason. Scientific Quarto notebooks aren't essays --- they're structured records of what you did, why you did it, and what happened. Bullet points force clarity. They prevent the wandering prose that makes notebooks unreadable six months later.

**Other conventions:**

-   Hashpipe (`#|`) comment formatting for all chunk options --- the modern Quarto way, not the old knitr `{r, echo=FALSE}` syntax
-   No numbered headings (Quarto's table of contents handles navigation)
-   Cross-references and figure captions follow Quarto's native citation system

### 4. writing-labarchive-entries 📓

This Skill targets LabArchives --- the electronic lab notebook platform many labs use for formal documentation.

**Four entry types, each with its own template:**

-   **Reference Datasets:** Where the data came from, how it was processed, what QC was applied
-   **Analysis Pipelines:** Step-by-step records of computational workflows
-   **Tool Evaluations:** Structured assessments of new tools or packages
-   **Educational Notes:** Learning records for new methods or concepts

**The PubMed integration:** This Skill uses the PubMed MCP to look up citations automatically. When Claude writes a lab entry referencing a method, it pulls the actual citation --- PMID, authors, journal, year --- rather than hallucinating a plausible-sounding reference.

### 5. developing-r-packages 📦

This Skill follows Wickham & Bryan's *R Packages* (2nd edition) as the authoritative reference. It covers the full lifecycle:

-   Package structure with `usethis` scaffolding
-   `roxygen2` documentation with proper `@param`, `@return`, `@export` tags
-   `testthat` testing with meaningful test names and edge case coverage
-   Semantic versioning (MAJOR.MINOR.PATCH)
-   GitHub Actions CI/CD for `R CMD check`
-   `pkgdown` sites for documentation hosting

This is the Skill I used when developing [BadranSeq](https://github.com/wolf5996/BadranSeq). Having Claude follow these conventions from the start meant the package was properly documented and tested from day one --- not retroactively patched months later.

## THE DEPENDENCY GRAPH 🔗

One thing I'm particularly proud of is how these Skills reference and build on each other:

    creating-analysis-projects
    ├── writing-r-code          (code chunk conventions)
    └── writing-qmd-scientific  (document structure)
        └── writing-r-code      (inline code patterns)

    writing-labarchive-entries  (standalone — references PubMed MCP)
    developing-r-packages       (standalone — R package conventions)

When Claude loads `creating-analysis-projects`, it automatically picks up the code conventions from `writing-r-code` and the document structure from `writing-qmd-scientific`. You don't configure this manually --- the Skills reference each other, and Claude follows the chain.

This means one unified system rather than five disconnected instruction sets. Change a convention in `writing-r-code` and it propagates everywhere that Skill is referenced.

## FORK AND MAKE THEM YOURS 🛠️

These Skills are deliberately opinionated. They reflect *my* workflow, *my* conventions, *my* preferred packages. That's the point --- generic Skills produce generic output.

But your workflow is different. Maybe you use `dittoSeq` instead of BadranSeq. Maybe your lab uses a different directory layout. Maybe you work with spatial transcriptomics instead of scRNA-seq.

**To make them yours:**

-   Fork the repository
-   Swap the author name and preferred packages
-   Adjust the directory conventions to match your lab
-   Modify the Context7 package list to cover your stack

The README has a full "Fork & Customize" section with specific instructions for each change.

## WHAT'S NEXT 🚀

These five Skills are a starting point. As workflows mature, new Skills get added. I'm actively working on Skills for spatial transcriptomics analysis, automated figure generation, and manuscript preparation.

If you build something useful on top of these, I'd love to hear about it. Open an issue, submit a PR, or just fork and go your own way. That's the beauty of open-sourcing institutional knowledge --- it compounds.

The full Claude series lives at [badran-elshenawy.netlify.app](https://badran-elshenawy.netlify.app) 🌐

