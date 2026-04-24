---
title: "Advanced scRNA-seq for Biologists – Post 5: PAGA — The Bird's Eye View"
author: "Badran Elshenawy"
date: 2026-04-17T09:00:00Z
categories: ["scRNA-seq", "Trajectory Analysis", "Bioinformatics", "Tutorial"]
tags: ["scRNA-seq", "single-cell", "trajectory analysis", "PAGA", "Scanpy", "Python", "cluster connectivity", "bioinformatics", "computational biology"]
description: "PAGA provides a cluster-level view of your data's topology before detailed trajectory inference. Learn when to use this exploratory approach."
slug: "advanced-scrnaseq-paga-birds-eye-view"
draft: false
output: hugodown::md_document
aliases:
  - /posts/advanced_scrnaseq_paga_birds_eye_view/
summary: "Before mapping every single-cell footpath, scout the terrain from above. PAGA shows you which clusters connect to which."
featured: true
rmd_hash: 2f652add79c61640

---

# Advanced scRNA-seq for Biologists --- Post 5: PAGA --- The Bird's Eye View! 🕸️

You've learned Monocle3 for detailed trajectories! You've used scVelo to add directionality! But sometimes, before diving into single-cell resolution analysis, you need to step back and ask a simpler question! 🤔

**Which clusters are actually connected to which?**

That's what PAGA does! It gives you the big picture --- a map of cluster relationships that helps you understand your data's overall topology before committing to detailed trajectory inference! 🗺️

## What Is PAGA? 🧠

PAGA stands for **Partition-based Graph Abstraction**! The name sounds intimidating, but the concept is straightforward! 💡

Instead of drawing trajectories through individual cells, PAGA asks: if I treat each cluster as a single node, which nodes should be connected? And how confident am I in each connection? 🤔

The output is a graph where:

- **Nodes** represent clusters (or cell types)
- **Edges** represent connectivity between clusters
- **Edge weights** indicate confidence in the connection

Think of it as a subway map! You don't see every passenger or every step they take! You see which stations connect to which, and that's often enough to understand the overall system! 🚇

## Why Use PAGA? 🎯

PAGA fills a specific niche in your trajectory analysis toolkit:

**Exploratory analysis!** Before running Monocle3 or detailed pseudotime inference, PAGA shows you the overall structure! Is your data one continuous trajectory? Multiple disconnected populations? A complex branching tree? PAGA answers this quickly! 🌳

**Robust to noise!** Because PAGA operates at the cluster level rather than single-cell level, it's less sensitive to outliers and technical noise! Individual cells may be misplaced, but cluster-level relationships tend to be stable! 💪

**Scales well!** PAGA handles large datasets efficiently! When you have 100,000+ cells, running detailed trajectory inference can be slow! PAGA gives you a quick overview first! ⚡

**Intuitive outputs!** The cluster connectivity graph is easy to interpret and present! Non-computational collaborators can immediately see which populations relate to which! 📊

## The PAGA Workflow ⚡

PAGA lives in the Python/Scanpy ecosystem! If you've been working in R with Seurat and Monocle3, this means converting your data --- but the payoff is worth it for exploratory analysis! 🐍

Here's the minimal workflow:

``` python
import scanpy as sc

# Load your data (or convert from Seurat via anndata)
adata = sc.read_h5ad("your_data.h5ad")

# Compute neighbors if not already done
sc.pp.neighbors(adata, n_neighbors=15)

# Run PAGA using your existing clusters
sc.tl.paga(adata, groups='leiden')  # or 'seurat_clusters' if imported

# Visualize
sc.pl.paga(adata, threshold=0.1)
```

That's it! Three functional lines and you have a cluster connectivity map! 🎉

## Understanding PAGA Output 📊

The PAGA plot shows clusters as nodes, positioned based on their relationships! Edges connect clusters that PAGA believes are related, with thicker edges indicating stronger connections! 🔗

**What to look for:**

**Strong edges between expected neighbors!** If you're studying hematopoiesis, you should see strong connections from HSCs to progenitors to mature lineages! If expected connections are missing, investigate why! 🔬

**Unexpected connections!** If two clusters connect that shouldn't biologically relate, that's worth investigating! Is there a transitional population you missed? Or is this a technical artifact? 🤔

**Disconnected components!** If clusters form separate islands with no connections, forcing a single trajectory through your entire dataset would be inappropriate! Analyze each component separately! ✂️

**Connection strength gradients!** Strong connections suggest robust relationships; weak connections suggest tentative relationships that may not hold up to detailed analysis! 📈

## The Threshold Parameter 🔧

When you call `sc.pl.paga(adata, threshold=0.1)`, the threshold controls which edges are displayed:

- **Lower threshold (0.05):** Shows more edges, including weaker connections
- **Higher threshold (0.3):** Shows only the strongest connections

Start with 0.1 as a default! If the graph looks too cluttered, raise the threshold! If too sparse, lower it! The underlying connectivity data doesn't change --- you're just filtering the visualization! 🎯

## When to Use PAGA ✅

**Early in your analysis!** Before running Monocle3 or detailed trajectory inference, run PAGA to understand the overall structure! It takes seconds and can save hours of misguided analysis! ⏱️

**Complex datasets!** When you have many clusters and unclear relationships, PAGA helps you see the forest before examining individual trees! 🌲

**Large datasets!** When cell numbers make detailed trajectory inference slow, PAGA provides quick insights! 💻

**Hypothesis generation!** PAGA results suggest where detailed trajectories might exist! "These three clusters are strongly connected --- let's subset and run Monocle3 on just these!" 💡

**Sanity checking!** If PAGA shows no connection between two clusters, forcing a trajectory through both is probably inappropriate! 🚩

## When PAGA Isn't Enough ⚠️

PAGA has limitations:

**No single-cell resolution!** PAGA tells you clusters connect, not how individual cells transition! For detailed pseudotime analysis, you still need Monocle3 or similar! 📊

**No directionality!** PAGA edges are undirected! It tells you clusters are connected, not which direction cells move! For directionality, combine with RNA velocity! 🏹

**No pseudotime values!** PAGA provides topology (structure) but not ordering! You won't get a numerical pseudotime from PAGA alone! ⏱️

**Cluster-dependent!** PAGA results depend on how you clustered your data! Different clustering resolutions give different PAGA graphs! This isn't a bug --- it's inherent to the approach --- but it means you should consider how clustering resolution affects your conclusions! 🤔

## Combining PAGA with Other Methods 🔗

PAGA works best as part of a multi-method approach:

**PAGA → Monocle3:** Use PAGA to identify which clusters form a trajectory, then subset those clusters and run Monocle3 for detailed pseudotime! 🎯

**PAGA → scVelo:** PAGA shows structure; scVelo adds directionality! Together, you know both what connects and which way cells are moving! 🌊

**PAGA for validation:** After running Monocle3, check whether your detailed trajectory respects PAGA's cluster connectivity! If Monocle3 connects clusters that PAGA shows as unrelated, be skeptical! 🧐

## Converting Data from R 🔄

If your data lives in Seurat and you want to use PAGA, you have options:

**Option 1: Save from Seurat, load in Scanpy**

``` r
# In R
library(SeuratDisk)
SaveH5Seurat(seurat_obj, filename = "data.h5Seurat")
Convert("data.h5Seurat", dest = "h5ad")
```

``` python
# In Python
adata = sc.read_h5ad("data.h5ad")
```

**Option 2: Use reticulate for hybrid workflows**

This lets you pass objects between R and Python in the same session, though it adds complexity! 🤷

For most users, saving to h5ad and loading in Python is the simplest approach! ✅

## Bottom line 🎯

PAGA is your reconnaissance mission! Before mapping every single-cell footpath, scout the terrain from above! See which regions connect! Identify where detailed exploration makes sense! 🗺️

It won't replace Monocle3 or scVelo --- it complements them! Use PAGA to understand overall structure, then deploy detailed methods where they'll be most informative! 💪

In Post 6, we bring everything together: how to choose between all these methods, troubleshoot common problems, and make practical decisions with real-world data! 🛠️

See you in Post 6! 🙏

