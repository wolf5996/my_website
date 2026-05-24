---
title: "From Tidyverse to Scverse – Post 7: GlassBox UMAP — Making Dimensionality Reduction Interpretable"
author: "Badran Elshenawy"
date: 2026-05-25T09:00:00Z
categories: ["Python", "scRNA-seq", "Bioinformatics", "Tutorial", "UMAP", "Dimensionality Reduction"]
tags: ["glassbox umap", "umap", "interpretable ml", "dimensionality reduction", "marimo", "uv", "scRNA-seq", "bioinformatics", "single-cell", "python", "visualization"]
description: "A walkthrough of GlassBox UMAP — a UMAP variant with per-point, per-feature contributions — built on uv and Marimo. Same modern stack, new layer of interpretability."
slug: "tidyverse-to-scverse-glassbox-umap"
draft: false
output: hugodown::md_document
aliases:
  - /posts/tidyverse_to_scverse_glassbox_umap/
summary: "Your pipeline is fast and reproducible. But the UMAP step is still a black box. GlassBox UMAP changes that."
featured: true
rmd_hash: 27ef29a004b7de2f

---

## The black box in your pipeline 🔒

In Post 6, I rebuilt the canonical PBMC 3k pipeline with a deliberately modern stack. Polars for wrangling. Plotnine for plotting. uv for environments. Marimo for notebooks. The pipeline works. It is fast, reproducible, and pleasant to run.

But there is a black box in the middle of it.

UMAP takes your high-dimensional data, performs some topological magic, and drops a beautiful 2D embedding into your lap. You look at it. You cluster it. You annotate cell types by where the dots land. And then someone asks the question you cannot answer:

> "Why is this cell *here* and not *there*?"

Classical UMAP has no answer. It is a black-box transformation. You feed it a matrix, you get coordinates back, and the relationship between the two is entirely opaque. In single-cell analysis, where every point is a cell and every dimension is a gene, that opacity is a real problem. You are looking at a landscape without a legend.

That is the gap GlassBox UMAP fills.

## What GlassBox UMAP actually does 🔍

GlassBox UMAP is a UMAP variant with one critical addition: it can tell you which features matter for every single point in the embedding.

After fitting, you call `compute_contributions()` and get back a tensor of shape `(n_samples, n_features, n_components)`. In plain language, that means for every cell, for every gene, for both UMAP dimensions, you have a number quantifying how much that gene pushed that cell in that direction.

This is not a post-hoc explanation. It is not SHAP or LIME bolted onto the side. The contributions are computed from the UMAP objective itself, so they are intrinsic to the embedding rather than approximated around it.

The result is an interpretable dimensionality reduction. You can overlay contribution maps directly onto the embedding and ask questions that were previously impossible:

- Which genes drive the separation between Cluster 1 and Cluster 2?
- Why does this outlier cell sit at the edge of the plot?
- If I remove a particular marker gene, how does the local geometry change?

For bioinformatics, this is compelling. The LinkedIn post that originally caught my attention showed exactly this: gene-by-cell contribution maps overlaid on a scRNA-seq UMAP, turning an abstract embedding into a biologically readable map.

## The stack you already know ⚡

I want to make something explicit before walking through the repo. Every tool in this project is a tool I have already introduced in this series.

| Layer         | Tool              | Introduced In     |
|---------------|-------------------|-------------------|
| Environment   | **uv**            | Post 3            |
| Notebook      | **Marimo**        | Post 5            |
| Analysis      | **GlassBox UMAP** | This post         |
| Baseline      | **UMAP**          | Standard practice |
| Preprocessing | **scikit-learn**  | Standard practice |

There is no new infrastructure here. The same `uv sync` and `uv run marimo edit` workflow from Post 6 gets you into an interactive notebook in seconds. The only new thing is the analysis layer --- GlassBox UMAP itself --- and that is the point.

This is what a mature modern stack feels like. You swap the analysis engine, everything else stays the same, and the developer experience does not degrade. That consistency is valuable. It means you can evaluate new tools without fighting your environment.

## The walkthrough 🍷

The repo is here: [github.com/wolf5996/glass-box-umap](https://github.com/wolf5996/glass-box-umap)

I chose the UCI Wine dataset for this walkthrough rather than jumping straight to single-cell data. The reason is structural. Wine has thirteen features that feed directly into UMAP. There is no PCA indirection, no latent space, no intermediate representation. Features go in, embedding comes out, contributions map cleanly back to the original features. It is the cleanest possible API demonstration.

The notebook follows a deliberate sequence:

**Setup and EDA**

Load the data, inspect feature distributions, and verify the class labels. Marimo renders the dataframe beautifully without any extra configuration.

**Feature scaling**

StandardScaler with explicit assertions that mean equals 0 and standard deviation equals 1. I have made this a habit in every pipeline --- assert your invariants, do not assume them.

**Classical UMAP baseline**

Fit `umap-learn` on the scaled data and visualise the embedding by cultivar class. This is the reference point. You need to see what the classical algorithm produces before you can evaluate whether GlassBox UMAP gives you something comparable.

**GlassBox UMAP embedding**

Fit `GlassBoxUMAP` with the same hyperparameters. The embedding looks similar --- that is the first sanity check. If the interpretable variant produced a completely different shape, you would have to choose between interpretability and fidelity. It does not. The geometry is preserved.

**Feature contributions**

Call `compute_contributions()` and inspect the tensor. For any point in the embedding, you can now query which features pushed it left or right, up or down. The contributions are signed and normalised, so you can rank them intuitively.

**Interactive visualisation**

Overlay the contribution maps onto the embedding using Bokeh. Hover over a point and see its top contributing features. Toggle between features and watch the landscape shift. This is where the black box becomes glass.

## Classical UMAP vs GlassBox UMAP

Here is the comparison at a glance:

| Property | Classical UMAP | GlassBox UMAP |
|----|----|----|
| Embedding quality | Excellent | Comparable |
| Speed | Fast | Slightly slower (contribution tracking) |
| Feature contributions | None | Native, per-point, per-feature |
| Interpretability | Black box | Glass box |
| API complexity | Standard UMAP | Identical + one extra call |
| scRNA-seq ready | Yes | With caveats (see below) |

The API is genuinely minimal. If you can fit classical UMAP, you can fit GlassBox UMAP. The only additional step is `compute_contributions()`, and the return value is a NumPy array you can slice, plot, or export however you like.

## The scRNA-seq caveat 🧬

I need to be honest about something, because the original LinkedIn demonstration showed GlassBox UMAP on single-cell data and that is where the biological excitement is.

In standard scRNA-seq workflows, UMAP is not fit on raw gene expression. It is fit on principal components. The feature contribution tensor returned by GlassBox UMAP reflects the features *actually passed to the algorithm*. If you pass PCs, you get contributions *per PC*, not per gene.

Recovering per-gene interpretability requires propagating those contributions back through the PCA loadings. That is mathematically straightforward --- multiply the PC contributions by the loadings matrix --- but as of this writing, the package does not handle that transparently. You would need to do it manually.

This notebook deliberately avoids that indirection by using the Wine dataset, where features map directly to contributions. A future extension to PBMC 3k will tackle the PCA pass-through explicitly, either by computing the back-propagation manually or by fitting GlassBox UMAP directly on the gene expression matrix (computationally expensive for 20,000+ genes, but conceptually clean).

The takeaway is not that GlassBox UMAP is unusable for scRNA-seq. The takeaway is that the current API gives you per-PC contributions, and per-gene contributions require one extra engineering step. That step is worth taking, but it is not automatic yet.

## Why this matters for the series 🎯

This post is not just a tool showcase. It is a stress test for the modern stack thesis I have been building across six posts.

The PBMC 3k pipeline in Post 6 proved that the stack works for a standard single-cell workflow. This repo proves that the same stack scales to a completely different domain --- interpretable dimensionality reduction --- without any infrastructure changes. Same `pyproject.toml` format. Same `uv.lock` semantics. Same Marimo reactive execution model. Same pleasant developer experience.

That is the real win. Not any individual tool, but the fact that the tools compose. You can swap Polars for pandas, Plotnine for seaborn, GlassBox UMAP for classical UMAP, and the container stays stable. The environment does not fracture. The notebook does not break. The workflow does not degrade.

For R users still watching this series with skepticism, this is the pattern I want you to notice. Two years ago, Python's data science tooling was fragmented. Today, uv + Marimo + a growing ecosystem of high-quality packages form a coherent, reproducible, pleasant workflow. The individual pieces were inspired by R's best ideas. The composition is uniquely Python's.

## Running it yourself

Three commands, same as every repo in this series:

``` bash
git clone https://github.com/wolf5996/glass-box-umap.git
cd glass-box-umap
uv sync
```

Then:

``` bash
# Interactive (recommended)
uv run marimo edit scripts/01_glass_box_umap_marimo.py

# Headless
uv run python scripts/01_glass_box_umap_marimo.py
```

Open the notebook, step through the cells, and hover over the interactive Bokeh plot. Watch the contribution maps update as you toggle between features. That is the moment the black box becomes glass --- and it is worth experiencing firsthand.

## What's next

This post introduced GlassBox UMAP as a concept and demonstrated it on a clean, direct-feature dataset. The obvious next step is scRNA-seq: taking the PBMC 3k data from Post 6, fitting GlassBox UMAP, and solving the PCA back-propagation problem so that per-gene contributions are available for biological interpretation.

That is coming. For now, clone the repo, run the notebook, and see what interpretable dimensionality reduction feels like when your embedding can actually explain itself.

See you in the next one.

