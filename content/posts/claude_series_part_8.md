---
title: "Claude Series – Post 8: Subagents - Your Terminal Now Has a Whole AI Team!"
author: "Badran Elshenawy"
date: 2026-03-06T09:00:00Z
categories:
  - "AI Tools"
  - "Bioinformatics"
  - "Productivity"
  - "Claude"
  - "Research Tools"
tags:
  - "Claude"
  - "Claude Code"
  - "Subagents"
  - "AI Agents"
  - "Parallel Processing"
  - "Custom Agents"
  - "AI Workflow"
  - "Bioinformatics"
  - "Computational Biology"
  - "Automation"
  - "Research Productivity"
  - "Agent Teams"
description: "Master Claude Code subagents - spawn parallel AI specialists that explore, test, review, and document your code simultaneously. Learn built-in agents, custom creation, and real bioinformatics workflows."
slug: "claude-subagents-ai-team-in-your-terminal"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/claude_subagents_ai_team_in_your_terminal/"
summary: "Stop working sequentially! Learn how Claude Code subagents let you spawn parallel AI specialists - each with its own context window and expertise - to explore, review, test, and document your bioinformatics code simultaneously."
featured: true
rmd_hash: 6aa86fa5bde5e26d

---

What if one Claude wasn't enough? 🤯

In Post 7, we gave Claude the ability to talk to the outside world through MCPs - databases, APIs, PubMed, GitHub, you name it! 🌍

Now we're going even further. Claude can talk to ITSELF - spawning specialized copies that work on different parts of your problem AT THE SAME TIME! ⚡

Welcome to subagents - the feature that turns your terminal into a full AI research team! 🧑‍🔬🧑‍🔬🧑‍🔬

## WHAT ARE SUBAGENTS? 🎯

Subagents are specialized AI assistants that your main Claude session can spawn to handle specific tasks! 🧬

Each subagent gets:

-   Its own context window (completely separate from your main conversation!)
-   A custom system prompt (specialized instructions for its job!)
-   Specific tool access (you control what it can and can't do!)
-   Independent permissions (read-only, full access, whatever you need!)

Think of it like a lab PI delegating to postdocs! 🏛️

You're the PI talking to Claude (the lab manager). Claude then assigns specific tasks to specialized subagents (the postdocs) who each go off, do their focused work, and come back with results!

The key insight: subagents work INDEPENDENTLY and return results to your main session. They don't clutter your main conversation with all the intermediate work! 🧹

## WHY NOT JUST ASK CLAUDE DIRECTLY? 🔬

This is the question everyone asks - and the answer is beautifully simple!

**Context window preservation** is the number one reason! 💡

Every file Claude reads, every search it runs, every error message it processes - all of that eats into your conversation's context window. Do enough exploration and your main session starts forgetting earlier instructions! 😰

Subagents solve this completely! They do all the heavy reading, searching, and processing in THEIR OWN context window and return ONLY the distilled summary you actually need! 🎯

Here's the difference in practice:

**Without subagents:**

    You: "Explore my codebase and then refactor the pipeline"
    Claude: *reads 50 files, context window fills up*
    Claude: *starts forgetting your refactoring preferences halfway through*

**With subagents:**

    You: "Explore my codebase and then refactor the pipeline"
    Claude: *spawns Explore subagent*
    Explore subagent: *reads 50 files in its own context*
    Explore subagent: *returns 20-line summary of relevant architecture*
    Claude: *uses summary to refactor with FULL context window available*

Same task. Dramatically better results! 🚀

## BUILT-IN SUBAGENTS YOU ALREADY HAVE 🤖

Claude Code ships with three built-in subagents that it uses automatically - you don't even need to set them up!

**Explore** 🧊

The read-only specialist. When Claude needs to search or understand your codebase without making changes, it delegates to Explore! This keeps all those file reads and grep results out of your main conversation!

Claude specifies a thoroughness level when invoking Explore: quick for targeted lookups, medium for balanced exploration, or very thorough for comprehensive analysis!

**Plan** 🌊

The research agent for plan mode. When you ask Claude to plan something and it needs to understand your project first, Plan goes out and gathers all the context! It prevents infinite nesting (subagents can't spawn other subagents) while still doing thorough research!

**General-purpose** 🕵️

The heavy lifter. When a task needs both exploration AND modification, complex reasoning to interpret results, or multiple dependent steps - General-purpose handles it! This is the subagent that takes on multi-step operations that would be too complex for a quick explore!

The beautiful part? Claude auto-delegates to these based on what your task needs! You just describe what you want and Claude picks the right specialist! ✨

## CREATING CUSTOM SUBAGENTS - BUILD YOUR DREAM TEAM 💪

This is where it gets REALLY powerful for researchers! 🔬

**The quick way - /agents command:**

``` bash
# In Claude Code, just type:
/agents
# Select "Create new agent"
# Choose User-level (all projects) or Project-level (this repo only)
# Select "Generate with Claude" and describe what you want!
```

Claude will generate the system prompt, suggest tool permissions, and even let you pick a color to visually distinguish your subagent! 🎨

**The manual way - markdown files:**

Subagents are defined as markdown files with YAML frontmatter. Drop them in:

-   `.claude/agents/` - project-level (shared with your team via git!)
-   `~/.claude/agents/` - user-level (available in ALL your projects!)

Here's what a custom subagent file looks like:

``` markdown
---
name: qc-reviewer
description: Reviews single-cell RNA-seq QC decisions including
  filtering thresholds, normalization choices, and batch effects.
  Use when validating analysis parameters.
tools:
  - Read
  - Grep
  - Glob
model: sonnet
---

You are a single-cell RNA-seq quality control specialist.

Review the analysis code and evaluate:
- nFeature_RNA and nCount_RNA filtering thresholds
- Mitochondrial percentage cutoffs
- Doublet detection parameters
- Normalization method appropriateness
- Batch correction decisions

Flag any thresholds that seem too aggressive or too lenient
based on the dataset characteristics. Always explain your
reasoning with reference to established best practices.
```

That's it! Save it and Claude will automatically delegate QC review tasks to this specialist! 🧪

## THE BIOINFORMATICS DREAM TEAM 🧬

Here's where I get excited. Imagine having these subagents ready to go for every project:

**🧪 QC-Reviewer**

Validates your scRNA-seq filtering thresholds, checks normalization choices, flags suspicious batch effects. Runs on Haiku for speed since it's read-only analysis!

**📊 Stats-Checker**

Reviews your DESeq2 model design matrices, checks for confounding variables, validates your contrast specifications. Catches the statistical mistakes that ruin publications!

**🎨 Figure-Formatter**

Enforces journal-specific figure requirements - DPI, dimensions, font sizes, color palettes. No more rejection emails over figure formatting!

**📝 Methods-Writer**

Reads your analysis code and drafts methods sections. Knows the difference between how you'd write methods for Nature vs Bioinformatics vs a preprint!

**🔍 Literature-Scout** (combine with MCPs from Post 7!)

Searches PubMed for relevant references using the PubMed MCP, then summarizes findings. Your own automated literature review assistant!

The real power? Run them ALL in parallel on the same project! ⚡

    You: "Review my scRNA-seq pipeline - check QC, stats, figures, 
          and draft the methods section"

    Claude spawns:
      → QC-Reviewer (checking filtering thresholds)
      → Stats-Checker (validating DESeq2 design)  
      → Figure-Formatter (checking plot specifications)
      → Methods-Writer (drafting from code)

    All four working SIMULTANEOUSLY! ⏱️
    Results back in minutes instead of running each sequentially!

## PARALLEL EXECUTION - THE SPEED MULTIPLIER 🏎️

Claude Code can run up to 7 subagents simultaneously! Here's how to trigger parallel execution:

    "Use 4 parallel tasks to explore my project:
     1. Scan the R scripts for package dependencies
     2. Check the Snakemake workflow for bottlenecks
     3. Review the output figures for consistency
     4. Summarize the README and documentation status"

Each task gets its own subagent, its own context window, and runs independently! The results come back as they finish!

For a large bioinformatics project with dozens of scripts, this turns a 20-minute sequential review into a 5-minute parallel sweep! 🚀

## SUBAGENTS VS SKILLS VS CLAUDE.MD - THE COMPLETE PICTURE 🗺️

With Posts 5-8 complete, here's how everything fits together:

**CLAUDE.md (Post 5)** = Your project's memory! 📋

Tells Claude about your project, coding standards, and architecture. Loads automatically every session!

**Skills (Post 6)** = Reusable instruction packages! 📦

Teaches Claude HOW to do specific tasks. Runs in your main conversation so you can iterate live!

**MCPs (Post 7)** = External connections! 🔌

Plugs Claude into databases, APIs, and tools. Expands WHAT Claude can access!

**Subagents (Post 8)** = Parallel specialists! 🧑‍🔬

Spawns focused helpers with their own context. Multiplies HOW MUCH Claude can do at once!

Together they form the complete Claude Code power stack:

CLAUDE.md provides context → Skills provide expertise → MCPs provide connectivity → Subagents provide parallelism! 🏗️

## PRO TIPS FOR EFFECTIVE SUBAGENTS ⚡

**1. Match the model to the task!**

Set `model: haiku` for simple read-only tasks (fast and cheap!) and keep `model: opus` for complex reasoning. A QC check doesn't need Opus - but a statistical review might!

**2. Restrict tools deliberately!**

A code reviewer should be read-only. A refactoring agent needs write access. Think about what each specialist SHOULD and SHOULDN'T do!

**3. Write clear descriptions!**

Claude uses the description field to decide when to delegate. "A helpful agent" tells Claude nothing. "Reviews DESeq2 model design for confounding variables and contrast specifications" is crystal clear!

**4. Keep subagent output concise!**

Subagent results come back into your main context. If they return massive outputs, you lose the context-saving benefit! Instruct them to return summaries, not raw data!

**5. Remember: subagents can't spawn subagents!**

One level of delegation only. Plan your task decomposition accordingly!

**6. Version control your agents!**

Store project-level agents in `.claude/agents/` and commit them to git. Your whole lab gets the same specialist team!

## GETTING STARTED CHECKLIST ✅

Ready to build your AI team? Here's your action plan:

1.  **Try the built-ins first** - Ask Claude to explore your codebase and watch it auto-delegate to the Explore subagent!

2.  **Create your first custom agent** - Run `/agents` in Claude Code and build a simple code reviewer!

3.  **Go parallel** - Ask Claude to use multiple tasks simultaneously on a real project!

4.  **Build domain specialists** - Create agents tailored to YOUR bioinformatics workflow!

5.  **Share with your team** - Commit `.claude/agents/` to your repo so everyone benefits!

## WHAT'S NEXT? 🔮

Over the past 8 posts, we've gone from "What even IS Claude?" to building parallel AI research teams in your terminal! 🚀

The Claude Code ecosystem is evolving fast - Agent Teams (experimental!) take this even further by letting subagents communicate WITH EACH OTHER instead of just reporting back to the main session!

Keep experimenting, keep building, and keep pushing what's possible with AI in research! 🧬

The full series lives at [badran-elshenawy.netlify.app](https://badran-elshenawy.netlify.app) 🌐

