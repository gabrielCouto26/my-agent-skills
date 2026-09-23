---
name: state-local
description: Use this skill when the user asks to generate, create, or update the project state JSON (state.local.json, or state.main.json / state.master.json via --main or --master) at the project root containing the current state of development.
---

# Instructions

Create or update a project state JSON file at the project's root. This file contains the current development state formatted as JSON, allowing future AI agents to quickly ingest the context, read essential documentation, and spend fewer tokens parsing raw text.

## Parameters

`--main` and `--master` are **aliases** with the same outcome: they activate **primary-remote mode**. If neither flag is passed, use **local mode**.

| Invocation | Mode | Source of truth | Output file |
|---|---|---|---|
| default (no flag) | local | working tree + current branch | `state.local.json` |
| `--main` or `--master` | primary-remote | `origin/main` or `origin/master` | `state.main.json` or `state.master.json` |

### Mode selection

1. If the user passed `--main` or `--master` (or clearly asked for main/master remote state), use **primary-remote mode**.
2. Otherwise, use **local mode**.

## Objective

Generate a machine-readable JSON snapshot of the project's state to serve as context for AI agents. It must highlight implemented features, current progress, next steps, and explicitly map important documentation files so the AI can easily find architectural and business context.

All modes share the same top-level `projectState` shape. Content fullness depends on the mode:

- **Primary-remote** and **standalone local** (no primary state file): full baseline snapshot.
- **Complementary local** (primary state file already exists): sparse delta that references the primary file and must not duplicate its content. Optional field `primaryStateFile` is required only in complementary local mode.

## Process — Local mode

### 0. Detect primary state file (before writing)

1. Look for `state.main.json` at the project root.
2. If missing, look for `state.master.json`.
3. If both exist, prefer `state.main.json`.
4. **If neither exists:** use **standalone local** (full snapshot steps below). Do **not** auto-run primary-remote mode to create a missing primary file.
5. **If one exists:** use **complementary local** (delta steps below). Read that primary JSON as baseline. Do **not** regenerate or rewrite the primary file in local mode.

### Standalone local (no `state.main.json` / `state.master.json`)

1. **Analyze the project**:
   - Browse through the folder structure (e.g., `apps/`, `packages/`, `src/`).
   - Check the git status (`git status`, current branch).
   - Identify main tests, endpoints, routes, and components.

2. **Locate Important Documentation (CRITICAL)**:
   - Find and list all key documentation files (e.g., `README.md`, `ARCHITECTURE.md`, `docs/CHAT_SUMMARY.md`, `docs/API.md`, ADSs, RFCs).
   - Briefly describe what each documentation file contains so the AI knows exactly which file to read for specific needs.

3. **Identify implemented features**:
   - List completed functionalities (e.g., screens, endpoints, hooks, services).
   - Include references to key files and conventions used.

4. **Identify where development stopped**:
   - What is currently in progress (branch, PR, incomplete feature).
   - Natural next steps to continue development.
   - Technical pending items (e.g., backend endpoints not implemented, missing tests).

5. **Generate the file** `state.local.json` in the root of the project (full schema content; do **not** include `primaryStateFile`).

### Complementary local (primary state file exists)

1. **Read the primary file** (`state.main.json` or `state.master.json`) and treat it as baseline project context.

2. **Analyze local-branch delta** relative to that baseline:
   - Check `git status` and the current local branch.
   - When helpful, compare against the matching primary short name (e.g. `git log main..HEAD`, `git diff main...HEAD`, or `master` equivalents).
   - Focus on **new or materially advanced implementations** on the local branch—not a full inventory of every changed file.

3. **Write a complementary `state.local.json`**:
   - Set `projectState.primaryStateFile` to `"state.main.json"` or `"state.master.json"` (the file that was found).
   - Set `projectName`, `lastUpdate`, `branch` (current local branch).
   - Set a short `overview` (one paragraph) stating this file is a local delta on top of the primary state file—do not restate the primary project overview.
   - `implementedFeatures`: only features new or materially advanced on the local branch. You **may go deeper** than the primary file on these items (`summary`, `notes`, `mainFiles`).
   - `whereDevelopmentStopped`: local WIP, next steps, and local-only technical debt.
   - `importantDocumentationFiles`, `endpointsAndAPIs`, `relevantFileStructure`, `usefulCommands`, `conventionsAndPatterns`: include **only** entries that are new, changed, or specifically relevant to the local delta; otherwise use empty arrays/objects. Never copy lists already recorded in the primary file.

4. **Do not duplicate** primary content (shared overview boilerplate, already-listed features, shared docs, conventions, commands, or endpoints already marked implemented on primary).

## Process — Primary-remote mode (`--main` / `--master`)

Do **not** check out `main` or `master`. Analyze the remote primary branch in place.

1. **Resolve the remote primary ref**:
   - Run `git fetch origin` when network/remote is available.
   - Prefer `origin/main` if that ref exists; otherwise use `origin/master`.
   - If neither `origin/main` nor `origin/master` exists after fetch, stop and tell the user. Do not invent a filename or fabricate remote state.
   - Let `<ref>` be the resolved remote ref (`origin/main` or `origin/master`) and `<short>` be `main` or `master` accordingly.
   - Write exactly one file: `state.<short>.json` (never both `state.main.json` and `state.master.json` in one run).

2. **Analyze the project at `<ref>`** (without checkout):
   - List the tree: `git ls-tree -r --name-only <ref>`.
   - Inspect recent history: `git log <ref> --oneline` (reasonable depth).
   - Read key files via `git show <ref>:<path>` (docs, package manifests, routes, endpoints, tests, components).
   - Identify main tests, endpoints, routes, and components **as they exist on `<ref>`**.

3. **Locate Important Documentation (CRITICAL)** on `<ref>`:
   - Find and list key documentation files present on that ref.
   - Briefly describe what each file contains so the AI knows which file to read for specific needs.

4. **Identify implemented features** on `<ref>`:
   - List completed functionalities as reflected by the remote primary branch.
   - Include references to key files and conventions used on that branch.

5. **Identify where development stopped** on the **remote primary branch** (not local WIP):
   - What appears in progress or incomplete **on `<ref>`**.
   - Natural next steps relative to that remote state.
   - Technical pending items visible on that branch.

6. **Generate the file** `state.<short>.json` in the root of the project.
   - Set `projectState.branch` to `<short>` (`main` or `master`).
   - Do **not** include `primaryStateFile` (that field is local complementary only).

## Expected Structure

Output ONLY valid JSON. Do not include markdown code blocks around the JSON in the final file content if writing directly to the file system.

### Full snapshot (primary-remote or standalone local)

Populate all sections with complete baseline context. Do not include `primaryStateFile`.

```json
{
  "projectState": {
    "projectName": "[Project Name]",
    "lastUpdate": "[YYYY-MM-DD]",
    "branch": "[branch-name]",
    "overview": "[1-2 paragraphs describing the project and its current high-level state]",
    "importantDocumentationFiles": [
      {
        "path": "[file-path, e.g., docs/ARCHITECTURE.md]",
        "purpose": "[Why an AI should read this file]"
      }
    ],
    "implementedFeatures": [
      {
        "name": "[Feature Name]",
        "summary": "[What was done]",
        "mainFiles": ["[path1]", "[path2]"],
        "notes": "[Conventions, mappings, constraints]"
      }
    ],
    "whereDevelopmentStopped": {
      "inProgress": "[Description of the current active task]",
      "nextSteps": [
        "[Step 1]",
        "[Step 2]"
      ],
      "pendingTechnicalDebts": [
        "[e.g., unimplemented endpoints, missing integrations]"
      ]
    },
    "endpointsAndAPIs": {
      "implemented": ["[list]"],
      "notImplemented": ["[list]"]
    },
    "relevantFileStructure": [
      "[simplified path of the most important folders/files]"
    ],
    "usefulCommands": {
      "install": "[command]",
      "build": "[command]",
      "test": "[command]",
      "lint": "[command]"
    },
    "conventionsAndPatterns": [
      "[Code conventions, tests, routes, etc.]"
    ]
  }
}
```

### Complementary local delta (when `state.main.json` or `state.master.json` exists)

Same shape, plus required `primaryStateFile`. Sparse sections for non-delta data; deeper detail allowed only for local new work.

```json
{
  "projectState": {
    "projectName": "[Project Name]",
    "lastUpdate": "[YYYY-MM-DD]",
    "branch": "[current-local-branch]",
    "primaryStateFile": "state.main.json",
    "overview": "[One short paragraph: this file is a local delta on top of primaryStateFile]",
    "importantDocumentationFiles": [],
    "implementedFeatures": [
      {
        "name": "[New or advanced local feature]",
        "summary": "[Deeper detail on local work]",
        "mainFiles": ["[path1]", "[path2]"],
        "notes": "[Local-only conventions, mappings, constraints]"
      }
    ],
    "whereDevelopmentStopped": {
      "inProgress": "[Local WIP]",
      "nextSteps": ["[Local next step]"],
      "pendingTechnicalDebts": ["[Local-only debt]"]
    },
    "endpointsAndAPIs": {
      "implemented": [],
      "notImplemented": []
    },
    "relevantFileStructure": [],
    "usefulCommands": {},
    "conventionsAndPatterns": []
  }
}
```

## Requirements

- **Local mode:** create/update `/state.local.json` from the working tree and current branch. Before writing, detect `state.main.json` then `state.master.json` and choose standalone vs complementary behavior.
- **Complementary local:** require `primaryStateFile`; do not duplicate primary content; focus on local new/advanced work; empty arrays/objects for non-delta sections.
- **Standalone local / primary-remote:** full snapshot; omit `primaryStateFile`.
- **Primary-remote mode:** create/update `/state.main.json` or `/state.master.json` from `origin/main` or `origin/master` (detection order above); set `branch` to the matching short name.
- Do not check out the primary branch to perform the analysis.
- Do not write both `state.main.json` and `state.master.json` in a single run.
- Do not rewrite the primary state file during local mode.
- The output MUST be 100% valid JSON. Ensure proper escaping of quotes and special characters.
- Prioritize clarity and conciseness.
- Emphasize the `importantDocumentationFiles` array when producing a full snapshot; in complementary mode, only list docs new or relevant to the local delta.
- Avoid duplicating the full content of existing documentation files; just reference them and explain their purpose.
- Use the current date in "lastUpdate".

## Output

Write the generated JSON directly into the mode-specific file (`state.local.json`, `state.main.json`, or `state.master.json`). Then reply to the user confirming it was generated successfully, naming the written file and which ref/branch was analyzed (and, in complementary local mode, which primary file was referenced), and summarizing the next steps found in the file.
