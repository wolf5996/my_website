# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Hugo personal website for Badran Elshenawy (bioinformatician, Oxford postdoc). Uses the Coder theme, deployed on Netlify. Contains 85+ blog posts across multiple tutorial series.

## Development Commands

```bash
hugo server -D    # Local dev server with drafts
hugo server       # Local dev server without drafts
hugo              # Production build (output to public/)
```

## Architecture

- **Config**: `hugo.toml` (site metadata, theme, social links, nav, taxonomies)
- **Deploy**: `netlify.toml` (Hugo v0.141.0)
- **Theme**: `themes/Coder/` (git submodule — do not edit directly)
- **Archetype**: `archetypes/default.md` (uses `+++` TOML front matter, but actual posts use `---` YAML)

## Content Conventions

### Dual-file pattern
Every blog post exists as **two files**: a `.rmd` source and a rendered `.md` file. Both are committed. The `.rmd` is the source of truth; the `.md` is generated via `hugodown::md_document`.

### Post naming
Files use `{series_name}_part_{N}.md` format with underscores:
- `scrna_seq_series_part_15.md`
- `claude_series_part_8.md`
- `bulk_RNA_Seq_part_10.md`
- `tidyverse_series_part_14.md`
- Standalone posts: `my_first_post.md`, `vs_code_quarto_visual_mode.md`

### Front matter (YAML with `---` delimiters)
Required fields for blog posts:
```yaml
title: "Series Name – Post N: Subtitle"
author: "Badran Elshenawy"
date: 2026-01-01T08:00:00Z
categories: [...]
tags: [...]
description: "..."
slug: "url-friendly-slug"
draft: false
output: hugodown::md_document
summary: "..."
rmd_hash: <hash>
```
Optional: `aliases`, `featured`

### Taxonomies
Four taxonomy types are configured: `categories`, `series`, `tags`, `authors`.

### Images
- Post-specific images: `content/posts/images/`
- Site-wide images (avatar, etc.): `static/images/`

### Hugo config notes
- `markup.goldmark.renderer.unsafe = true` — raw HTML is allowed in markdown (needed for Rmd rendering)
- Pagination: 6 posts per page
- Color scheme: `auto` with toggle enabled

## Commit Message Convention

Lowercase, descriptive format: `{series} post {N}: {topic}`
Examples: `claude series post 8: subagents`, `scRNA-Seq series post 15: pea theory`

## Deployment

Auto-deploys to Netlify on push to `main`. Live at: https://badran-elshenawy.netlify.app/
