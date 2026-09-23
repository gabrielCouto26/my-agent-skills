# state-local - Doc.md
- **Link para o prompt "plain"**: [SKILL.md](./SKILL.md)
- **Objetivo**: Generate or update `state.local.json` (local branch) or `state.main.json` / `state.master.json` (via `--main` / `--master`) at the project root for development context.
- **Observações gerais**: Helps new agents understand implemented features, progress, and next steps. `--main` and `--master` are aliases that analyze `origin/main` or `origin/master` and write the matching `state.<main|master>.json` file. Without a flag, analyzes the local working tree into `state.local.json`. If `state.main.json` or `state.master.json` already exists, local mode writes a complementary delta: it sets `primaryStateFile`, avoids duplicating primary content, and focuses on new/advanced local work (with deeper detail on those items only).
- **Últimas atualizações**: 2026-09-23
