---
title: "Claude Series – Post 7: MCPs - Claude Just Learned To Talk To EVERYTHING!"
author: "Badran Elshenawy"
date: 2026-02-13T09:00:00Z
categories:
  - "AI Tools"
  - "Bioinformatics"
  - "Productivity"
  - "Claude"
  - "Research Tools"
tags:
  - "Claude"
  - "Claude Code"
  - "MCP"
  - "Model Context Protocol"
  - "PubMed"
  - "AI Workflow"
  - "Bioinformatics"
  - "Computational Biology"
  - "Automation"
  - "Research Productivity"
  - "API Integration"
  - "Scientific Literature"
description: "Master MCP (Model Context Protocol) - the universal standard connecting Claude to PubMed, GitHub, databases, and any external tool. Learn how to set up MCP servers and build a complete research command center from your terminal."
slug: "claude-mcp-connect-to-everything"
draft: false
output: hugodown::md_document
aliases:
  - "/posts/claude_mcp_connect_to_everything/"
summary: "Stop copy-pasting between tools! Learn how MCP turns Claude into a fully connected research assistant that can search PubMed, manage GitHub repos, access Google Drive, and interact with your entire research ecosystem - all from one terminal."
featured: true
rmd_hash: 03f2278221af472d

---

## The Problem: Claude Lives in a Bubble 🫧

In [Post 6](https://badran-elshenawy.netlify.app/posts/claude-skills-turn-claude-into-your-specialist/), we gave Claude **Skills** - reusable instruction packages that teach it HOW to do specific tasks like an expert. That was a massive upgrade!

But here's the thing - Skills make Claude smarter about your work, yet Claude still can't *reach out* and interact with external tools. Want to search PubMed? You have to do it yourself, copy the results, and paste them in. Need a file from Google Drive? Same thing. Want to post results to Slack? You're the middleman.

**MCPs change everything.** 🚀

MCP stands for **Model Context Protocol** - an open standard created by Anthropic that gives Claude hands to reach out and interact with the world. If Skills are Claude's brain training, MCPs are Claude's arms and legs.

------------------------------------------------------------------------

## What Is MCP, Really? 🔌

Think of MCP as **USB-C for AI**.

Before USB-C, every device had its own proprietary cable. Phones, cameras, hard drives - all different connectors. It was a mess.

Before MCP, every AI integration was custom. Want Claude to access your database? Build a custom integration. Connect to PubMed? Another one. Each connection required specific code, authentication handling, and maintenance.

MCP solves this with **one universal protocol** that connects Claude to *anything*. One standard, one interface, infinite connections.

Here's the architecture in plain English:

-   **MCP Client** = Claude (the AI asking for things)

-   **MCP Server** = The bridge to an external tool (PubMed, GitHub, Slack, etc.)

-   **Protocol** = The standardized language they use to communicate

When Claude needs to search PubMed, it sends a request through the MCP client to the PubMed MCP server. The server queries PubMed's API, gets results, and sends them back to Claude. All standardized, all automatic.

And here's the best part: **MCP is open source and model-agnostic.** OpenAI adopted it for ChatGPT in March 2025. Google confirmed support for Gemini. Microsoft integrated it into Copilot. This isn't a niche Anthropic feature - it's becoming the industry standard. 🌐

------------------------------------------------------------------------

## MCP Servers That Matter for Researchers 🔬

There are now over **1,200 MCP servers** available. Here are the ones that will transform your research workflow:

**Scientific Literature:**

-   **PubMed MCP** - Search 35M+ biomedical articles directly from Claude. Get abstracts, metadata, citations, even full-text when available. No more browser tab juggling!

-   **bioRxiv/medRxiv MCP** - Access preprints before they hit journals

-   **Semantic Scholar MCP** - Citation network analysis and paper recommendations

-   **arXiv MCP** - For those computational methods papers

**Development & Collaboration:**

-   **GitHub MCP** - Review pull requests, create issues, manage repos, search code. Your entire codebase management from Claude's terminal

-   **Slack MCP** - Send analysis summaries, share results, post updates to channels

-   **Google Drive MCP** - Pull shared documents, access collaborator files

-   **Notion MCP** - Manage lab notebooks and project documentation

**Data & Infrastructure:**

-   **Filesystem MCP** - Deep file system access and organization

-   **PostgreSQL MCP** - Query databases directly with natural language

-   **Docker MCP** - Manage containerized analysis environments

**Bioinformatics-Specific:**

-   **Ensembl MCP** - Genomic annotations and variant data

-   **UniProt MCP** - Protein sequences and functional information

-   **Reactome MCP** - Pathway data for biological interpretation

-   **PubChem MCP** - Chemical compound data and drug information

The biotech MCP ecosystem is growing fast. You can literally build a setup where Claude has direct access to your entire research infrastructure! 🧬

------------------------------------------------------------------------

## Setting Up Your First MCP Server ⚡

Here's what blew my mind - the setup is absurdly simple.

**For Claude Code (terminal), it's one command:**

``` bash
# Add PubMed MCP server
claude mcp add --transport stdio pubmed -- npx @cyanheads/pubmed-mcp-server

# Add GitHub MCP server
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github

# Add filesystem access
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /path/to/your/project
```

That's it. Claude auto-discovers the new tools and they're immediately available! ✨

**For HTTP-based servers (remote services):**

``` bash
# Connect to Notion
claude mcp add --transport http notion https://mcp.notion.com/mcp

# Connect to Asana
claude mcp add --transport sse asana https://mcp.asana.com/sse
```

**For Claude Desktop, edit the config file:**

On macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`

On Windows: `%APPDATA%\Claude\claude_desktop_config.json`

``` json
{
  "mcpServers": {
    "pubmed": {
      "command": "npx",
      "args": ["@cyanheads/pubmed-mcp-server"],
      "env": {
        "NCBI_API_KEY": "your_api_key_here"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/yourname/research"
      ]
    }
  }
}
```

Restart Claude Desktop and the servers start automatically! 🚀

------------------------------------------------------------------------

## Where MCP Configurations Live 📂

Just like Skills had multiple locations, MCP configs have a clear hierarchy:

-   **Project-level**: `.mcp.json` in your project root (version-controlled, shared with team)

-   **Project-local**: `.claude/settings.local.json` (personal project overrides, gitignored)

-   **User-level**: `~/.claude/settings.local.json` (your personal servers across all projects)

**Pro tip:** Put shared research MCPs (PubMed, GitHub) in your user-level config so they're always available. Put project-specific database connections in `.mcp.json` so your whole team gets them! 🤝

------------------------------------------------------------------------

## Real Research Workflow: The MCP Dream Setup 🧠

Let me show you what a fully connected research session looks like.

**The old way (without MCPs):**

1.  Open browser, go to PubMed, search for papers ⏱️
2.  Read abstracts, note relevant ones 📝
3.  Open another tab, go to GitHub, check collaborator's latest code 💻
4.  Switch to Google Drive, find shared analysis notes 📁
5.  Go back to terminal, tell Claude about what you found 🗣️
6.  Claude generates results, you copy them 📋
7.  Open Slack, paste summary for your PI 💬

**The MCP way:**

    You: "Search PubMed for the 5 most recent MERFISH liver zonation papers,
          check our GitHub repo for any related marker gene lists,
          cross-reference with my current DESeq2 results in ~/research/liver_project/,
          draft an introduction paragraph citing the relevant papers,
          and post a summary to #lab-updates on Slack"

**Claude does ALL of this.** One prompt. Multiple tools. Complete output. 🎯

The context switching alone probably saves 30-45 minutes per session. But the real magic is Claude's ability to **cross-reference** across sources - finding connections between literature, your code, and your data that you might have missed! 💡

------------------------------------------------------------------------

## Three Transport Types Explained 🔧

MCP servers communicate using three transport methods. Here's when to use each:

**stdio (Standard Input/Output):**

-   Server runs as a local process on your machine
-   Best for: Tools that need direct system access (filesystem, local databases)
-   Most common type for development tools

**HTTP:**

-   Server runs remotely, communicates over HTTPS
-   Best for: Cloud-based services (Notion, hosted APIs)
-   Recommended for most remote MCP servers

**SSE (Server-Sent Events):**

-   Similar to HTTP but with real-time streaming
-   Best for: Services that push updates (Asana, real-time dashboards)
-   Legacy transport being replaced by HTTP in newer implementations

For most researchers, you'll use **stdio** for local tools and **HTTP** for cloud services. Don't overthink it - the `claude mcp add` command handles the details! ⚡

------------------------------------------------------------------------

## Context Window Considerations ⚠️

One important thing to understand: every MCP server's tool descriptions consume tokens from Claude's context window. More servers = less room for your actual conversation.

**Best practices:**

-   Keep your active MCP servers to **2-8** at a time

-   Claude Code has **Tool Search** (enabled by default) that dynamically loads tool descriptions only when relevant, rather than preloading everything

-   If you notice Claude getting confused or forgetting earlier parts of your conversation, you might have too many servers active

-   Use project-level configs to activate only the MCPs relevant to that specific project

------------------------------------------------------------------------

## Building Your Own MCP Server 🛠️

Here's where it gets really exciting for bioinformatics. If a public MCP server doesn't exist for your tool, **you can build one**.

The MCP SDK is available in TypeScript and Python. A basic server needs:

1.  Define your **tools** (what actions Claude can take)
2.  Implement the **handlers** (what happens when Claude calls each tool)
3.  Set up the **transport** (stdio for local, HTTP for remote)

Imagine building MCP servers for:

-   Your lab's internal LIMS database
-   A custom variant annotation pipeline
-   Your institution's HPC job submission system
-   A local reference genome browser

Claude could literally submit Slurm jobs, check queue status, and pull results - all from natural language commands! The MCP SDK documentation is excellent, and Claude itself is great at helping you build MCP servers (very meta, I know 😄).

------------------------------------------------------------------------

## Skills + MCPs: The Complete Picture 🧩

Here's how the pieces fit together in the Claude ecosystem:

| Feature                | What It Does                          | Analogy             |
|---------------------|------------------------------|---------------------|
| **CLAUDE.md** (Post 5) | Tells Claude about your project       | Onboarding document |
| **Skills** (Post 6)    | Teaches Claude HOW to do tasks        | Training manual     |
| **MCPs** (Post 7)      | Gives Claude access to external tools | Work equipment      |

Together, Claude knows your project context (CLAUDE.md), knows your preferred methods and standards (Skills), and can reach out to interact with the world (MCPs).

That's not a chatbot anymore. That's a research assistant. 🧬

------------------------------------------------------------------------

## Getting Started Checklist ✅

Ready to set up your MCP-powered research environment? Here's your action plan:

1.  **Install your first MCP server** - Start with PubMed if you're in bioinformatics. One command, immediate value.

2.  **Add GitHub** - If you use version control (and you should!), this is a no-brainer.

3.  **Configure filesystem access** - Let Claude see your project directories for cross-referencing.

4.  **Try a multi-tool prompt** - Ask Claude something that requires combining information from multiple sources. Watch the magic happen.

5.  **Explore the ecosystem** - Browse [mcp-awesome.com](https://mcp-awesome.com) or the [official MCP GitHub](https://github.com/modelcontextprotocol) for servers relevant to your work.

------------------------------------------------------------------------

## What's Next? 🔮

We've now covered the core Claude Code power trio: CLAUDE.md for context, Skills for expertise, and MCPs for connectivity.

**Next up in Post 8: Subagents** - where Claude learns to spawn parallel AI helpers that work simultaneously on different parts of your task. One agent reads your codebase, another writes tests, another reviews code - all at the same time! 🤖

The series continues at [badran-elshenawy.netlify.app](https://badran-elshenawy.netlify.app) 🚀

