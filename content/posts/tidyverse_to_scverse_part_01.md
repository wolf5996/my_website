---
title: "From Tidyverse to Scverse – Post 1: Why I'm Making the Switch to Python"
author: "Badran Elshenawy"
date: 2026-04-24T09:00:00Z
categories: ["Python", "Bioinformatics", "scRNA-seq", "Tutorial", "Series Introduction"]
tags: ["Python", "R", "tidyverse", "scverse", "scanpy", "single-cell", "bioinformatics", "computational biology", "polars", "uv", "quarto", "data science"]
description: "Why an R-native bioinformatician is expanding into Python and scverse — and how this series will help you make the same transition."
slug: "tidyverse-to-scverse-why-switching"
draft: false
output: hugodown::md_document
aliases:
  - /posts/tidyverse_to_scverse_why_switching/
summary: "After 33 posts in R and building my own package, I'm adding Python to the toolkit. Here's why — and what's coming next."
featured: true
rmd_hash: fb2cbb5b2c0f1f73

---

## The Confession

I need to come clean about something.

I've spent the last year and a half building an entire educational ecosystem around R. Seventeen posts on bulk RNA-seq with DESeq2. Sixteen on scRNA-seq with Seurat. I built my own R visualization package --- BadranSeq --- because Seurat's default plots weren't cutting it for publication. I've written custom Claude Code Skills for R package development, scientific Quarto documents, and analysis projects.

R has been my home. And honestly? It still is.

But I've been watching the single-cell field evolve, and I can't ignore what's happening on the Python side anymore.

## What Changed

The scverse ecosystem happened.

If you've been following single-cell genomics over the past couple of years, you've noticed a pattern: the most exciting new tools are Python-first. Not Python-also. Not Python-eventually. Python-*first*.

Here's what I mean:

- **CellTypist:** automated cell type annotation using machine learning models trained on massive reference datasets. No real R equivalent with the same breadth of pre-trained models.

- **scVI / scANVI:** deep learning-based integration and annotation that handles batch effects with a sophistication that traditional methods struggle to match. Built entirely on PyTorch and anndata.

- **scVelo:** RNA velocity analysis that predicts future cell states from splicing dynamics. The R wrappers exist, but the core engine is Python.

- **CellRank:** fate probability estimation combining RNA velocity with transcriptomic similarity. Again, Python-native, anndata-native.

These aren't niche tools. They're becoming standard in high-impact single-cell papers. And they all speak the same language: anndata objects, scanpy preprocessing, scverse conventions.

The anndata object is the key to understanding why this ecosystem works so well. If you've used Seurat, think of anndata as the Python equivalent of the Seurat object --- but with a design philosophy that's closer to how SummarizedExperiment works in Bioconductor. It stores your expression matrix, cell metadata, gene metadata, dimensional reductions, and analysis results in a single, HDF5-backed structure that plays nicely with every tool in the scverse stack.

That interoperability is the real selling point. In R, moving data between Seurat, SingleCellExperiment, and various Bioconductor tools often means conversion steps that lose information or introduce subtle bugs. In the scverse world, your anndata object flows seamlessly from scanpy to scVI to scVelo to CellRank. Same object, no conversion, no data loss.

And the community behind scverse is genuinely impressive. It's not one lab's passion project --- it's a coordinated ecosystem with shared standards, joint documentation, and active governance. That kind of infrastructure matters when you're choosing tools for a three-year PhD project.

## Why This Isn't an "R vs Python" Series

Let me be crystal clear: I'm not abandoning R. I'm not here to tell you dplyr is worse than pandas (it isn't --- and we'll talk about that). I'm not going to pretend that ggplot2 doesn't produce better figures than matplotlib with a fraction of the effort.

This series exists because the reality of modern computational biology is bilingual. The best researchers I know use R for some things and Python for others, and they switch between them without drama.

But here's the problem --- most "learn Python for bioinformatics" resources assume you're starting from zero. They teach you what a variable is. They walk you through `print("hello world")`. That's not what you need.

What you need is a translation guide. You already think in tidy data. You already understand grouped operations. You already know what a good analysis pipeline looks like. You just need someone to show you where all that knowledge maps in Python --- and where Python genuinely does something R can't.

That's what this series is.

## The Pain Points This Series Addresses

If you're an R user who's dipped a toe into Python, you've probably hit at least one of these walls:

- **conda:** You tried to set up an environment, spent 45 minutes resolving dependencies, and ended up with a broken installation that somehow conflicts with your system Python. You went back to R.

- **pandas:** You wrote a chain of `.groupby().agg().reset_index()` and thought "this is worse than dplyr in every way." You weren't entirely wrong.

- **Jupyter notebooks:** You opened a .ipynb file in a text editor, saw raw JSON with base64-encoded images, and wondered why anyone would choose this over RMarkdown.

Every one of these pain points has a modern solution that didn't exist (or wasn't mature) two years ago. That's the whole thesis of this series.

The Python ecosystem has been quietly undergoing a revolution in developer experience. The tools that made Python painful for R users --- conda's glacial dependency resolution, pandas' unintuitive API, Jupyter's binary notebook format --- are being replaced by alternatives that are faster, cleaner, and in some cases genuinely better than what R offers. The R community pioneered a lot of these ideas (tidy evaluation, literate programming, reproducible environments), and now the Python ecosystem is catching up with its own implementations that sometimes take the concepts even further.

## The Roadmap

Here's what's coming:

- **Post 2: Polars vs Pandas:** Polars is a DataFrame library that will feel eerily familiar if you love the tidyverse. Method chaining, lazy evaluation, expressive syntax. We'll compare the same operations side by side: dplyr, pandas, and polars.

- **Post 3: uv vs conda:** uv is a package and environment manager written in Rust that makes conda feel like it's from 2010. Fast installs, deterministic resolution, no more "solving environment" for 20 minutes.

- **Post 4: qmd vs ipynb:** Quarto now has native Python support, which means you can write reproducible notebooks in plain text markdown instead of JSON. Better for version control, better for LLM-assisted coding, better for your sanity.

- **Posts 5+: The Real Stuff:** Deep dives into scanpy, CellTypist, scVelo, and more. Not toy examples --- real analysis pipelines on real data, with the biological reasoning front and centre.

## Who This Series Is For

You're in the right place if:

- You're an R user who wants to access Python-native single-cell tools without starting from scratch

- You're a wet lab researcher who's heard "just use scanpy" and wants to know what that actually means in practice

- You're a computational biologist who uses R day-to-day but keeps hitting walls when collaborators send you anndata files or when a reviewer asks "did you try scVI for integration?"

- You're curious about whether Python's ecosystem has caught up to R's ergonomics (spoiler: in some areas, it's surpassed them)

- You're tired of maintaining two separate mental models for data analysis and want a unified workflow that leverages the best of both languages

## Let's Go

This is going to be a fun ride. We're not converting --- we're expanding.

The goal is simple: by the end of this series, you should be able to set up a Python environment in under a minute, wrangle data in polars without missing dplyr, write reproducible notebooks in Quarto, and run a scanpy pipeline from raw counts to cell type annotations. All without abandoning the R tools that already work for you.

Same educational philosophy as always: biology first, code second, and every post should leave you able to do something you couldn't do before.

Next up: **Polars vs Pandas** --- the DataFrame library that makes tidyverse users feel right at home in Python.

See you there.

