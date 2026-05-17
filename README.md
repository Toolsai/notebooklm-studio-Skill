# NotebookLM Studio Skill

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

[繁體中文 README](README.zh-Hant.md)

Turn Codex into your NotebookLM research operator: prepare sources, create or reuse NotebookLM notebooks, generate NotebookLM Studio artifacts, download the outputs locally, and hand everything back to Codex for analysis, rewriting, study guides, decks, reports, and content workflows.

> NotebookLM Studio Skill does not replace NotebookLM. It connects NotebookLM to an AI agent workflow so Codex can help you operate the research process end to end.

---

## What It Does

NotebookLM Studio Skill helps you turn source material into a complete research workflow:

- Create or reuse NotebookLM notebooks
- Organize and add NotebookLM-supported sources such as web pages, PDFs, documents, slide decks, tables, audio, video, and images
- Trigger NotebookLM Studio artifacts
- Download generated outputs to a local folder
- Create handoff files so Codex can read and analyze the results directly
- Launch a local Dashboard control room
- Convert NotebookLM Mind Map JSON into interactive HTML
- Turn NotebookLM outputs into reports, tutorials, presentation outlines, content strategies, or research summaries

In short:

```text
Sources
→ NotebookLM notebook
→ NotebookLM Studio artifacts
→ Local downloads
→ Codex analysis, rewriting, and follow-up production
```

---

## Screenshots

### Codex + NotebookLM Studio Side by Side

![Codex and NotebookLM Studio side by side](docs/images/codex-dashboard-side-by-side.png)

Codex can read handoff files and analyze NotebookLM outputs on one side, while the NotebookLM Studio Dashboard manages notebooks, sources, recipes, and generation jobs on the other.

### Notebook Library

![Notebook library](docs/images/dashboard-library.png)

The Library view lets you browse NotebookLM notebooks, source counts, generated artifacts, recent activity, and readiness status.

### Notebook Data Room

![Notebook data room](docs/images/dashboard-data-room.png)

The Data Room is the workspace for a single notebook: sources on the left, goal-based workflows in the center, and native NotebookLM tools on the right.

### One-Click Workflows

The One-Click Workflows package common research jobs into ready-made flows. Instead of manually generating each artifact one by one, choose a goal and let the Skill create a useful set of NotebookLM outputs.

| Workflow | Best for |
| --- | --- |
| Executive Briefing Suite | Decision-ready briefing report, slide deck, data table, and audio overview. |
| Visual Story Suite | Turning research into video overview, slide deck, infographic, and mind map. |
| Learning Masterpack | Study guide, quiz, flashcards, mind map, and audio overview for learning. |
| Competitive Intel Suite | Custom report, data table, slide deck, and infographic for market or competitor research. |
| Due Diligence Review Suite | Custom report, data table, mind map, and quiz for review workflows. |
| Content Distribution Suite | Blog report, video overview, infographic, and slide deck for publishing. |
| Research Synthesis Suite | Research report, mind map, data table, and audio overview for multi-source synthesis. |
| Stakeholder Q&A Suite | Briefing report, slide deck, quiz, and flashcards for meetings or alignment. |
| Knowledge Base Digest Suite | Digest report, data table, mind map, and audio overview for knowledge bases. |

### Job Timeline and Codex Handoff

![Job timeline and Codex handoff](docs/images/job-timeline-handoff.png)

The timeline tracks generation status, artifact type, downloaded outputs, and the handoff path back into Codex.

### Generated Slide Deck / PDF + Codex Analysis

![Slide deck PDF display with Codex analysis](docs/images/slide-deck-pdf-display.png)

NotebookLM-generated slide decks or PDFs can be downloaded locally and reviewed side by side with Codex analysis.

---

## How Is This Different From Using NotebookLM Directly?

NotebookLM already supports many source formats and can generate useful outputs. This Skill adds the workflow layer around it.

| Need | NotebookLM directly | With this Skill |
| --- | --- | --- |
| Create notebooks | Manual | Assisted by Codex |
| Add many sources | Manual, one by one | Batch organization and checks |
| Generate artifacts | Manual clicking | Prompt or Dashboard driven |
| Download outputs | Manual | Downloaded into local folders |
| Analyze with Codex | Copy and organize yourself | Handoff files are created automatically |
| Mind Map JSON | Raw JSON | Interactive HTML conversion |
| Research tracking | Manual notes | Job timeline and manifest |
| Multi-step workflows | Human-operated | Codex + NotebookLM handoff loop |

Think of NotebookLM as the research model that reads a large body of material. This Skill is the operator layer that prepares sources, runs the workflow, downloads outputs, and hands them back to Codex.

---

## Installation

You can install the Skill in either of these ways.

### Option 1: `npx skills add`

If your agent or Skills manager supports `skills add`, run:

```bash
npx skills add Toolsai/notebooklm-studio-Skill
```

This is the simplest option for users already working with Skills.

### Option 2: `git clone`

Clone the repository:

```bash
git clone https://github.com/Toolsai/notebooklm-studio-Skill.git
```

Then place the folder where your agent can read Skills.

For Codex:

```bash
mkdir -p ~/.codex/skills
cp -R notebooklm-studio-Skill ~/.codex/skills/notebooklm-studio
```

For Claude Code:

```bash
mkdir -p ~/.claude/skills
cp -R notebooklm-studio-Skill ~/.claude/skills/notebooklm-studio
```

For other agents, use the Skills directory required by that tool.

---

## Quick Start

After installation, ask Codex:

```text
Initiate /notebookLM studio
```

Codex will follow the Skill instructions, check the environment, confirm NotebookLM availability, and guide you through Google login or authorization when needed. Login and security verification must be completed by the user.

---

## Launch the Dashboard

Ask your agent:

```text
Launch the NotebookLM Studio dashboard.
```

The agent will provide a local URL, for example:

```text
http://localhost:8765/
```

If the default port is already in use, the server will use the next available local port and report the actual URL.

---

## Prompt Examples

### Create a Research Notebook

```text
Use /notebookLM studio to create a new NotebookLM notebook called "AI Agent Research Pack", add the following URLs as sources, generate a Report and Mind Map, download the outputs, and analyze them for me.
```

### Turn Sources Into a Study Pack

```text
Use /notebookLM studio to turn these sources into a study pack: Report, Quiz, Flashcards, and Mind Map. Download all outputs and summarize the key ideas.
```

### Generate a Slide Deck

```text
Use /notebookLM studio to create a Slide Deck from this NotebookLM notebook. Download PDF and PPTX if available, then give me a presentation outline.
```

### Generate an Audio Overview

```text
Use /notebookLM studio to generate an Audio Overview from this notebook. Download it and tell me what topics it covers.
```

### Generate a Mind Map and HTML View

```text
Generate a NotebookLM Mind Map, download the JSON, convert it into interactive HTML, and give me both file links.
```

### Search Sources and Build a NotebookLM Pack

```text
Search for the most reliable OpenAI Codex Agent tutorials, prioritize official documentation and high-quality guides, create a new NotebookLM notebook, add the sources, then use NotebookLM to generate a deep tutorial report.
```

---

## Supported NotebookLM Studio Artifacts

| Artifact | Use |
| --- | --- |
| Report | Briefing, study guide, blog post, or custom report |
| Mind Map | Concept graph, with optional interactive HTML conversion |
| Audio Overview | Podcast-style audio summary |
| Video Overview | NotebookLM video summary |
| Slide Deck | Presentation deck |
| Infographic | Visual summary |
| Quiz | Assessment questions |
| Flashcards | Study cards |
| Data Table | Structured extraction or comparison table |

---

## Supported Sources

This Skill prioritizes source formats supported by NotebookLM, including PDF, TXT, Markdown, DOCX, CSV, PPTX, EPUB, audio, video, and images.

Its main role is source organization, import checks, generation tracking, output downloads, handoff creation, and follow-up Codex analysis.

---

## Use Cases

### Research Assistant

```text
Find sources → build NotebookLM → generate Report → Codex performs deeper analysis
```

### Study Coach

```text
Course material → Study Guide → Quiz → Flashcards → Codex designs practice
```

### Content Factory

```text
Sources → NotebookLM Report / Mind Map → Codex rewrites into posts, scripts, or decks
```

### Team Knowledge Base

```text
Docs / PRs / meeting notes → NotebookLM → Data Table / Summary / Action Plan
```

---

## Compatibility

This Skill is designed primarily for Codex workflows, especially local file handling, Dashboard operation, handoff analysis, and multi-step research tasks.

Other agents that support Skills or local tool execution can adapt the project, but may require platform-specific adjustments.

---

## Safety Boundaries

Use this project only with accounts, data, and content you are authorized to access.

- Do not use it to bypass paywalls, DRM, login walls, private access controls, or content restrictions
- Do not commit private login material, sessions, tokens, or sensitive local outputs to GitHub
- Google login and security verification must be completed by the user
- NotebookLM is a Google product. This project is not affiliated with or endorsed by Google

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
