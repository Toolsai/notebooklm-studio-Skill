---
name: notebooklm-studio
description: Operate Google NotebookLM from Codex for authorized source ingestion, notebook setup, source-grounded Q&A, and Studio artifacts such as Audio Overviews, Video Overviews, reports, mind maps, quizzes, flashcards, slide decks, infographics, and data tables.
---

# NotebookLM Studio

## Overview

Use this skill when the user wants Codex to turn files, URLs, YouTube links, notes, transcripts, Drive-style sources, or research material into NotebookLM notebooks and NotebookLM Studio outputs. The goal is to make Codex act as the operator: prepare sources, create or reuse notebooks, generate official artifacts where available, download outputs, and explain limitations clearly.

This skill relies on the `notebooklm` CLI from `notebooklm-py` when available. That CLI is unofficial and uses undocumented NotebookLM endpoints, so every workflow must probe the environment before assuming it can run.

## Operating Rules

1. Only ingest sources the user owns, provided, or is allowed to use. Do not bypass paywalls, DRM, private access controls, or copyright restrictions.
2. Prefer official NotebookLM outputs over local reconstructions. If an output is not exposed by the CLI or account plan, create a source-grounded fallback and label it as derived.
3. Keep artifacts traceable: return notebook title/id when known, source count, generated artifact names, local download paths, and any failed/skipped outputs.
4. For current feature availability, limits, and export formats, load `references/notebooklm-capabilities.md`.
5. For exact CLI commands and supported automation flags, load `references/notebooklm-py-cli.md`.
6. For mixed source handling and "any source" requests, load `references/source-ingestion.md`.
7. For output mapping and fallback decisions, load `references/artifact-strategy.md`.


## First-Time Initialization

When a new user asks to initialize, set up, or start using this skill, make the setup as automatic as possible. The user should not be asked to manually configure package paths, folders, profiles, or commands unless an error requires it.

1. Run the bootstrap check:

```bash
python scripts/bootstrap_notebooklm.py --json --print-guide
```

2. If `notebooklm` is missing, ask for permission to install dependencies, then run:

```bash
python scripts/bootstrap_notebooklm.py --install --print-guide
```

3. If authentication is missing or expired, ask the user to complete the Google login step, then run:

```bash
python scripts/bootstrap_notebooklm.py --login --auth-test --print-guide
```

The login browser is the manual boundary: Codex can start it, but the user must choose the Google account, pass any MFA, and grant access. After login returns, Codex should run the environment check again and tell the user whether NotebookLM is ready.

4. Immediately after initialization, respond with a concise but complete guide. Prefer the generated guide from `bootstrap_notebooklm.py --print-guide`; load `references/quickstart-guide.md` if a fuller tutorial is useful.

5. If the user wants the local control room UI, start the dashboard from the current project/workspace:

```bash
python scripts/dashboard_server.py --host 127.0.0.1 --port 8765 --profile default --out-dir ./notebooklm_outputs/dashboard
```

Then give the user the local URL. If port `8765` is occupied, choose the next available local port and report the actual URL.

## Standard Workflow

1. Clarify the deliverable only if needed: audience, language, artifact types, output folder, and whether to create a new notebook or reuse an existing one.
2. Run an environment check:

```bash
python scripts/validate_environment.py --json
```

If `notebooklm` is missing, explain that NotebookLM automation needs `notebooklm-py` plus Google authentication. Ask before installing packages or opening browser login.

3. Build a source manifest before uploading:

```bash
python scripts/source_manifest.py --output /tmp/notebooklm-sources.json SOURCE...
```

Review warnings for unsupported extensions, over-large files, too many sources, missing files, paywalled URLs, or YouTube videos without usable captions.

4. Create a dry-run plan with the optimized artifact pipeline:

```bash
python scripts/artifact_pipeline.py plan --notebook-title "Project Title" --source SOURCE --artifact audio --artifact video --artifact mind-map --artifact slide-deck --download --out-dir ./outputs
```

5. Run only after the plan looks correct and authentication is ready:

```bash
python scripts/artifact_pipeline.py run --notebook-title "Project Title" --source SOURCE --artifact audio --artifact video --artifact mind-map --artifact slide-deck --download --out-dir ./outputs --status-file ./outputs/notebooklm-jobs.json
```

6. Verify outputs: list artifacts, check local files exist, inspect file sizes, and summarize what was generated.

## Optimized Artifact Pipeline

For multi-artifact requests, prefer `scripts/artifact_pipeline.py` over the older linear orchestrator. It avoids the "audio must finish before video starts" problem by submitting requested artifacts with `--no-wait` where NotebookLM supports it, then polling a shared job table. Completed artifacts are downloaded immediately; failed artifacts do not block unrelated outputs.

Use this pattern for requests like "make audio, video, mind map, and slides":

```bash
python scripts/artifact_pipeline.py run \
  --notebook-title "Research Pack" \
  --source ./report.pdf \
  --source https://example.com/article \
  --artifact audio \
  --artifact video \
  --artifact mind-map \
  --artifact slide-deck \
  --download \
  --download-slide-format both \
  --convert-mind-map-html \
  --status-file ./notebooklm_outputs/research-pack-jobs.json \
  --out-dir ./notebooklm_outputs
```

The pipeline should report each artifact as `planned`, `submitted`, `in_progress`, `completed`, `downloaded`, `failed`, `submit_failed`, `download_failed`, or `timeout`. Do not let a failed video prevent audio, mind-map, or slide-deck delivery.

## Local Dashboard

This skill includes a portable local dashboard under `dashboard/`, served by `scripts/dashboard_server.py`. Use it when the user asks for a visual NotebookLM Studio control room, wants to browse notebooks, launch common artifact workflows, inspect generation status, or hand completed outputs back to Codex.

Start it from the user's active project folder so outputs are stored with that project:

```bash
python scripts/dashboard_server.py \
  --host 127.0.0.1 \
  --port 8765 \
  --profile default \
  --out-dir ./notebooklm_outputs/dashboard
```

Dashboard behavior:

- Lists notebooks, sources, native NotebookLM artifacts, and recent generated jobs.
- Provides nine goal-based workflow recipes plus the nine NotebookLM native Studio tools.
- Submits real NotebookLM CLI jobs in the background and downloads completed artifacts.
- Writes handoff files and a plain-text `latest_agent_prompt.md` for Codex analysis.
- Uses manual prompt copy fallback: it opens a selected prompt panel and asks the user to press Command+C / Command+V when browser clipboard access is blocked.

Dashboard safety and portability:

- Bind to `127.0.0.1` by default. Do not bind to `0.0.0.0` unless the user explicitly understands the network exposure.
- Keep generated outputs under `--out-dir`; the default is the current workspace's `notebooklm_outputs/dashboard`.
- Treat the dashboard as an operator UI for the user's authorized NotebookLM account, not as a public service.

## Mind Map Visualization

NotebookLM downloads mind maps as JSON. When a user asks for a mind map, treat the JSON as the canonical official artifact, then automatically create an interactive local HTML view:

```bash
python scripts/mindmap_html.py ./notebooklm_outputs/mind-map.json -o ./notebooklm_outputs/mind-map.html
```

`artifact_pipeline.py` does this automatically when `--convert-mind-map-html` is enabled, which is the default. Return both files: JSON for traceability, HTML for human use.

## Artifact Defaults

Use these artifact names with `scripts/nblm_orchestrator.py --artifact` and the `notebooklm generate` CLI:

- `audio`: Audio Overview / podcast-style summary.
- `video`: Video Overview.
- `cinematic-video`: cinematic Video Overview when the user has access.
- `report`: NotebookLM report; use `--report-format briefing-doc`, `study-guide`, `blog-post`, or `custom` when appropriate.
- `mind-map`: mind map artifact.
- `quiz`: interactive quiz.
- `flashcards`: flashcard deck.
- `slide-deck`: NotebookLM slide deck.
- `infographic`: NotebookLM infographic.
- `data-table`: structured table export.

For requested outputs such as deep-analysis JSON, editorial scripts, translated briefs, or custom tables that are not direct Studio artifacts, use `notebooklm ask --json` or a generated report as the grounding layer, then create a derived local artifact with citations and mark it as derived.

## User-Facing Result Contract

When the workflow finishes, answer with:

- What notebook was used or created.
- What sources were added and which were skipped.
- Which outputs are official NotebookLM artifacts and which are derived by Codex.
- Local artifact paths, using absolute paths when files exist.
- Any account-plan limits, age-gated features, authentication failures, or known NotebookLM inaccuracies that affected the result.

Do not claim a generated artifact exists until it has been downloaded or verified in NotebookLM.
