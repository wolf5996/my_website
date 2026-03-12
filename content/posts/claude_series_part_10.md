---
title: "Claude Series – Post 10: Superpowers - The Plugin That Changes Everything!"
author: "Badran Elshenawy"
date: 2026-03-10T09:00:00Z
categories:
  - "AI Tools"
  - "Bioinformatics"
  - "Productivity"
  - "Claude"
  - "Research Tools"
tags:
  - "Claude"
  - "Claude Code"
  - "Superpowers"
  - "Obra"
  - "TDD"
  - "AI Workflow"
  - "Bioinformatics"
  - "Computational Biology"
  - "Automation"
  - "Software Development"
  - "Agentic Coding"
  - "Open Source"
description: "Master Superpowers by obra - the Claude Code plugin with 40,000+ GitHub stars that enforces brainstorming, planning, TDD, and code review into every AI coding session. Transform Claude from code generator to senior developer."
slug: "claude-superpowers-plugin-that-changes-everything"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/claude_superpowers_plugin_that_changes_everything/"
summary: "Stop letting Claude dump unplanned, untested code! Superpowers enforces a complete software development methodology - brainstorm, plan, test-first implementation, and continuous review - transforming your AI coding workflow permanently."
featured: true
rmd_hash: 85a40e853609339d

---

In [Post 9](https://badran-elshenawy.netlify.app/posts/claude-plugins-app-store-for-your-terminal/), we explored the Claude Code plugin ecosystem - 9,000+ extensions that transform your terminal into an infinite toolbox! 📦

Now I want to spotlight the ONE plugin that fundamentally changed how I work with Claude Code! 🤯

[Superpowers](https://github.com/obra/superpowers) by [obra (Jesse Vincent)](https://github.com/obra) isn't just another plugin. It's a complete software development METHODOLOGY that turns Claude from an eager code generator into a disciplined senior developer! 🦸

75,000+ GitHub stars. Officially accepted into the [Anthropic marketplace](https://code.claude.com/docs/en/discover-plugins). And once you try it, you'll understand why! ⭐

## THE PROBLEM SUPERPOWERS SOLVES 🎯

Be honest - you've experienced this! 😅

You ask Claude to build something. Claude IMMEDIATELY starts writing code. No questions asked. No planning phase. No tests. Just a massive code dump that might work, might not, and definitely doesn't follow any structured methodology!

The result? Incomplete features, untested edge cases, spaghetti architecture, and hours spent debugging code that should have been planned properly from the start! 😩

Superpowers fixes this by making Claude REFUSE to skip steps! It enforces a professional workflow that mirrors how senior engineers actually work! 💪

## INSTALLATION - 30 SECONDS TO TRANSFORMATION 🚀

Two commands:

``` bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

Restart Claude Code and you'll see a new injected prompt confirming Superpowers is active! 🦸

From this moment, Claude has access to a library of composable skills that enforce structured development! You can also trigger them manually with slash commands like `/superpowers:brainstorm` or just ask conversationally - "Use superpowers to help me brainstorm this task" works perfectly!

## THE SUPERPOWERS WORKFLOW - PHASE BY PHASE 🔬

This is where it gets beautiful. Superpowers implements a complete development lifecycle in seven phases:

**Phase 1: Brainstorming** 🧠

Instead of jumping to code, Claude activates the brainstorming skill! It asks YOU questions - Socratic-style - to refine your idea before a single line gets written!

    /superpowers:brainstorm I want to build a QC pipeline
    for our 10X Chromium scRNA-seq data

Claude will ask about:

-   What datasets are you working with?
-   What QC metrics matter most for your analysis?
-   Any existing pipelines to integrate with?
-   What should the output format look like?
-   Edge cases: mixed species? Multiplexed samples?

This catches bad assumptions BEFORE they become bad code! In my experience, 10 minutes of brainstorming saves HOURS of rework! ⚡

**Phase 2: Planning** 📋

Once requirements are clear, Claude breaks the work into micro-tasks:

    /superpowers:write-plan

Each task is scoped to 2-5 minutes of work! The plan is written to a markdown file that you can review, edit, reorder, and approve before implementation begins!

This is crucial - you maintain FULL CONTROL over what gets built and in what order! No more Claude deciding to refactor your entire project when you asked for one new function! 😤

**Phase 3: Implementation with TDD** 🧪

This is the core discipline. Superpowers enforces the classic Red-Green-Refactor TDD cycle:

1.  **Red** - Write a failing test FIRST
2.  **Green** - Write minimal code to make it pass
3.  **Refactor** - Clean up while keeping tests green
4.  **Commit** - Checkpoint your progress

If Claude tries to write implementation code before tests, Superpowers literally makes it DELETE the code and start over! No exceptions! 🔴🟢🔄

**Phase 4: Continuous Review** 🔍

Between every task, the requesting-code-review skill activates automatically! It reviews work against the plan and reports issues by severity:

-   **Critical** - blocks progress until fixed
-   **Major** - should be addressed before moving on
-   **Minor** - noted for later

It's like having a senior engineer doing continuous code review on every commit! 👨‍💻

**Phase 5: Subagent-Driven Development** 🤖

Superpowers leverages the subagents from [Post 8](https://badran-elshenawy.netlify.app/posts/claude-subagents-ai-team-in-your-terminal/)! Implementation runs in a two-stage review process:

1.  A subagent implements the task
2.  First review: does it match the spec?
3.  Second review: is the code quality high?

This double-review catches both "wrong thing built right" AND "right thing built wrong"! 🎯

**Phase 6: Git Workflow** 🌿

Superpowers manages git worktrees for clean branch management! Each feature gets its own isolated branch, keeping your main branch pristine!

**Phase 7: Shipping** 🚢

When all tasks are complete, the finishing skill verifies tests pass and presents options:

-   Create a pull request
-   Merge to main locally
-   Keep working
-   Discard the branch

Clean, professional, organized! ✨

## THE SKILLS THAT MAKE IT WORK ⚡

Superpowers is built on composable skills - each one teaching Claude a specific discipline:

**brainstorming** - Activates before writing code. Refines rough ideas through questions, explores alternatives, presents design in sections for validation!

**planning** - Structures work into reviewable micro-tasks with clear acceptance criteria!

**test-driven-development** - Enforces Red-Green-Refactor with no shortcuts allowed!

**requesting-code-review** - Automatic review with severity-based issue reporting!

**subagent-driven-development** - Fast iteration with two-stage review (spec compliance, then code quality)!

**debugging** - Four-phase methodology: reproduce, investigate root cause, fix, verify. No "let me just try random things" approach!

**writing-skills** - Meta-skill! Teaches Claude how to create NEW skills, so you can extend Superpowers with your own domain expertise!

The writing-skills skill is particularly powerful - it means you can teach Claude to teach itself! Feed it a methodology from a paper or book and it'll package it as a reusable skill! 🤯

## SUPERPOWERS FOR BIOINFORMATICS 🧬

I want to be specific about why this matters for our field, because "TDD for bioinformatics" might sound abstract:

**Building analysis pipelines:**

Without Superpowers: Claude writes a monolithic Snakemake workflow, misses edge cases, testing is an afterthought!

With Superpowers: Claude brainstorms your data characteristics, plans modular rules, writes tests for each step (does rule X produce expected output for known input?), reviews for reproducibility, and ships a pipeline that WORKS! 🐍

**Developing R packages:**

Without Superpowers: Claude dumps a bunch of functions, maybe adds a vignette, no systematic testing!

With Superpowers: Claude plans the API surface, writes testthat tests for each function FIRST, implements to pass tests, reviews for CRAN compliance, and produces a package that passes R CMD check! 📦

**Automating repetitive analyses:**

Without Superpowers: Claude writes a script that works on YOUR data but breaks on everything else!

With Superpowers: Claude brainstorms edge cases (empty gene lists? Wrong organism? Missing columns?), plans defensive code, tests each failure mode, and produces automation that's actually robust! 🛡️

## SUPERPOWERS + THE CLAUDE CODE STACK 🗺️

Here's how Superpowers fits with everything we've covered in Posts 5-10:

**CLAUDE.md** tells Claude about your project → **Skills** teach Claude HOW to work → **MCPs** connect to external tools → **Subagents** provide parallelism → **Plugins** package it all → **Superpowers** enforces the DISCIPLINE to use it all properly!

Superpowers is the capstone - it's not just another capability, it's the framework that makes ALL the other capabilities produce professional-quality results! 🏛️

## GETTING STARTED CHECKLIST ✅

Ready to transform your Claude Code workflow?

1.  **[Install Superpowers](https://github.com/obra/superpowers#installation)** - Two commands and a restart!

2.  **Start with brainstorming** - Next time you want to build something, try `/superpowers:brainstorm` and see how different the process feels!

3.  **Trust the process** - The first time Claude asks you 10 questions before writing code, it feels slow. By the third project, you'll never go back!

4.  **Extend with your own skills** - Use the writing-skills skill to create domain-specific superpowers for your bioinformatics workflows!

5.  **Share with your lab** - Install at the project level and commit to git. Your whole team gets the methodology!

## THE BIGGER PICTURE 🔮

Over 10 posts, we've gone from "What even IS Claude?" to a complete AI-powered research workflow:

Understanding Claude (Posts 1-2) → Terminal power with Claude Code (Posts 3-4) → Project context with CLAUDE.md (Post 5) → Reusable expertise with Skills (Post 6) → External connectivity with MCPs (Post 7) → Parallel specialists with Subagents (Post 8) → An infinite ecosystem with Plugins (Post 9) → Professional discipline with Superpowers (Post 10)!

This is just the beginning. The Claude Code ecosystem evolves weekly - Agent Teams, new plugins, community-built skills - the toolbox keeps expanding! 🚀

Keep experimenting. Keep building. Keep pushing what's possible with AI in research! 🧬

The full series lives at [badran-elshenawy.netlify.app](https://badran-elshenawy.netlify.app) 🌐

