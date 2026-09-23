---
name: state-local
description: Use this skill when the user asks to generate, create, or update the state.local.json file at the project root containing the current state of development.
---

# Instructions

Create or update the `state.local.json` file in the project's root. This file contains the current development state formatted as JSON, allowing future AI agents to quickly ingest the context, read essential documentation, and spend fewer tokens parsing raw text.

## Objective

Generate a machine-readable JSON snapshot of the project's state to serve as context for AI agents. It must highlight implemented features, current progress, next steps, and explicitly map important documentation files so the AI can easily find architectural and business context.

## Process

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

5. **Generate the file** `state.local.json` in the root of the project.

## Expected Structure of state.local.json

Output ONLY valid JSON. Do not include markdown code blocks around the JSON in the final file content if writing directly to the file system. Use the following schema:

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

## Requirements

- The file must be created at `/state.local.json` (root of the project/monorepo).
- The output MUST be 100% valid JSON. Ensure proper escaping of quotes and special characters.
- Prioritize clarity and conciseness.
- Emphasize the `importantDocumentationFiles` array; this is crucial for the AI's agility.
- Avoid duplicating the full content of existing documentation files; just reference them and explain their purpose.
- Use the current date in "lastUpdate".

## Output

Write the generated JSON directly into the `state.local.json` file. Then, reply to the user confirming it was generated successfully and summarizing the next steps found in the file.
