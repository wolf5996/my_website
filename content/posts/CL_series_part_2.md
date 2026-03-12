---
title: "Essential CLI Tools You Should Know - Part 2: zoxide & ouch"
author: "Badran Elshenawy"
date: 2024-12-18
categories: ["CLI Tools", "Linux", "Productivity"]
tags: ["CLI", "Linux", "SysAdmin", "DevOps", "Automation"]
description: "A deep dive into two essential CLI tools, `zoxide` and `ouch`, that will enhance your workflow."
slug: "cli-tools-zoxide-ouch"
draft: false
output: hugodown::md_document
aliases:
  - /posts/CL_series_part_1/
summary: "Explore `zoxide`, a better `cd`, and `ouch`, a modern `tar` replacement to improve your Linux workflow."
featured: true
rmd_hash: a2518de793853ab7

---

# 🛠️ Essential CLI Tools You Should Know - Part 2: `zoxide` & `ouch`

The command line is **powerful**, but it can also be **tedious**. Navigating deep directory structures or handling compressed files can slow down even the best workflows. Fortunately, two **modern CLI tools** solve these pain points:

-   **`zoxide`** → A smarter, faster way to `cd`
-   **`ouch`** → The easiest way to compress & extract files

------------------------------------------------------------------------

## 🌟 `zoxide` -- A Smarter `cd`

Tired of typing long, repetitive paths? `zoxide` **learns your most-used directories** and lets you jump to them instantly. No more `cd ../../../../some/deep/folder` nonsense!

### 🔹 Why `zoxide`?

✅ **Learns your habits** -- The more you use it, the smarter it gets  
✅ **Fuzzy matching** -- Jump to directories without typing the full path  
✅ **Integrates with all shells** -- Works with Bash, Zsh, Fish, and more  
✅ **Super fast** -- Finds directories instantly

### ⚡ Quick Start

#### 📥 Install:

-   **Ubuntu/Debian:** `cargo install zoxide`  
-   **MacOS:** `brew install zoxide`

#### 💡 Common Usage:

``` bash
# Replace `cd` with `z`
zoxide init bash | source  # Or `zoxide init zsh | source`

# Jump to frequently used directories
z project  # Navigates to ~/Documents/project if used frequently

# List most-used directories
zoxide query
```

🔥 **Why use it?** `zoxide` **remembers your habits**, making navigation effortless!

------------------------------------------------------------------------

## 🌟 `ouch` -- The Ultimate Compression & Extraction Tool

Dealing with compressed files shouldn't require memorizing dozens of flags. `ouch` simplifies **archiving and extracting** into a single, intuitive command.

### 🔹 Why `ouch`?

✅ **Universal syntax** -- One command for all archive formats  
✅ **Multi-file compression** -- Compress multiple files at once  
✅ **Auto-detects formats** -- No need to specify `.tar.gz`, `.zip`, `.7z` manually  
✅ **Works everywhere** -- Cross-platform and written in Rust

### ⚡ Quick Start

#### 📥 Install:

-   **Ubuntu/Debian:** `cargo install ouch`  
-   **MacOS:** `brew install ouch`

#### 💡 Common Usage:

``` bash
# Extract any compressed file
ouch x archive.zip  # Extracts `archive.zip`, detecting format automatically

# Compress multiple files
ouch c file1.txt file2.txt archive.tar.gz  # Creates `archive.tar.gz`

# List archive contents
ouch l archive.zip
```

🔥 **Why use it?** No more juggling `tar`, `gzip`, `unzip`, or `7z`---`ouch` **does it all**!

------------------------------------------------------------------------

## ✅ Final Thoughts

Both `zoxide` and `ouch` **simplify everyday terminal tasks**, making CLI life much easier.

### 🔥 Pro Tips:

🔸 **Make `zoxide` the default `cd`** by adding this to `~/.bashrc` or `~/.zshrc`:

``` bash
alias cd="z"
```

🔸 **Use `ouch` for quick file transfers** -- No need to manually extract archives first!

🚀 **These tools will save you countless keystrokes and headaches!**

------------------------------------------------------------------------

### 📢 Want more?

🔗 Check out my **full blog here**: [badran-elshenawy.netlify.app/posts](https://badran-elshenawy.netlify.app/posts/)

#CLI #Linux #Productivity #SysAdmin #DevOps #Automation

