# Tools and Runtimes Preferences

Preferred tools, command styles, model/runtime habits, and CLI conventions.

## Shell Commands in AiPlus Workspace

- Always prefix with `rtk` for shell commands in AiPlus workspace.
- Examples: `rtk git status`, `rtk cargo test`, `rtk npm run build`.
- Use `rtk proxy <cmd>` only when raw command output is needed.

## AiPlus CLI Patterns

| Command | Purpose |
|---------|---------|
| `aiplus profile status` | Check profile loading status |
| `aiplus profile doctor` | Diagnose profile issues |
| `aiplus status` | General AiPlus status |
| `aiplus doctor` | General AiPlus health check |
| `aiplus compact prepare` | Prepare for context compaction |
| `aiplus compact checkpoint` | Create checkpoint before compact |
| `aiplus compact resume` | Resume after compact |
| `aiplus secret-broker status` | Secret broker health |
| `aiplus secret-broker doctor` | Secret broker diagnosis |
| `aiplus secret-broker list` | List approved aliases |
| `aiplus update all` | Update AiPlus ecosystem |

## Secret Broker Usage

- Use `aiplus secret-broker run --aliases openai,kimi,deepseek -- <command...>` for explicit runtime secret needs.
- Prefer explicit aliases so unused placeholder providers do not block unrelated commands.
- Do not print, paste, log, summarize, compact, or persist secret values.
- If secret access is unavailable, report one exact fix command.

## Bitwarden

- Real smoke checks require `bws` CLI and read-only machine account.
- If `bws` unavailable, use mock/local metadata checks only.
- Never ask for, print, paste, or store secret values.

## Model / Runtime Preferences

- Kimi Code: `base_url=https://api.kimi.com/coding/v1`, `model=kimi-for-coding`.
- Kimi Open Platform / Moonshot: separate key system, do not reuse `kimi` alias.
- Default output for secret resolution: metadata only, no secret IDs or values printed.

## Auto Compact

- Before long-running work: `aiplus compact validate`, then `aiplus compact checkpoint`.
- Suggest compact only after checkpoint is ready.
- After compact: auto-resume if host gives control back; otherwise accept natural continuation phrases.

## Auto Team Consultant

- Use for: advice, CEO orchestration, review, implementation handoff, team discussion, prompt review, workflow tiering, Owner gate judgment.
- Advisor session: Advisor mode, LIGHT by default, no file edits unless explicitly approved.
- CEO session: CEO mode, set goal, decompose tasks, use agents, require Result Packets.
- Reviewer session: findings first, PASS/REVISE/BLOCKED.
- Builder session: changed files, verification, risks, review request.
