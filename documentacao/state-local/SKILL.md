---
name: state-local
description: Use this skill when the user asks to generate, create, or update the State.local.md file at the project root containing the current state of development.
---

# Instructions

Create or update the `State.local.md` file in the project's root, containing the current development state so that future AI agents can quickly contextualize themselves and spend fewer tokens.

## Objective

Generate a snapshot of the project's state to serve as context for new AI agents: implemented features, where the process stopped, and what is left to do. This way, agents will have more precise answers and generate fewer corrections.

## Process

1. **Analyze the project**:

   - Browse through the folder structure (e.g. `apps/`, `packages/`)
   - Check existing documentation files (e.g. `docs/CHAT_SUMMARY.md`, `README.md`)
   - Check the git status (`git status`, current branch)
   - Identify main tests, endpoints, routes, and components

2. **Identify implemented features**:

   - List completed functionalities (e.g., screens, endpoints, hooks, services)
   - Include references to key files and conventions used

3. **Identify where development stopped**:

   - What is in progress (branch, PR, incomplete feature)
   - Natural next steps
   - Technical pending items (e.g., backend endpoints not implemented)

4. **Generate the file** `State.local.md` in the root of the monorepo

## Expected Structure of State.local.md

```markdown
# Project State – [Project Name]

> Last update: [date]
> Branch: [branch-name]

## Overview

[1-2 paragraphs describing what the project is and its current state at a high level]

## Implemented Features

### [Feature Name 1]

- **What was done**: [summary]
- **Main files**: [paths]
- **Notes**: [conventions, mappings, constraints]

### [Feature Name 2]

...

## Where Development Stopped

- **In progress**: [description]
- **Next steps**: [ordered list]
- **Pending**: [e.g., unimplemented endpoints, missing integrations]

## Endpoints / APIs (Current State)

- **Implemented**: [list]
- **Not implemented**: [list if any]

## Relevant File Structure

[simplified path of the most important folders/files]

## Useful Commands

```bash
# [commands for build, lint, test]
```

## Conventions and Patterns

- [Code conventions, tests, routes, etc.]
```

## Requirements

- The file must be created at `/State.local.md` (root of the monorepo)
- Prioritize clarity and conciseness
- Include only information that a new agent would need to resume work
- Avoid repeating documentation that already exists in other files (reference them instead of duplicating)
- Use the current date in "Last update"

## Output

Create the `State.local.md` file, and then confirm to the user that it was generated and where it is located.
