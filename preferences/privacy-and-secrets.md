# Privacy and Secrets Preferences

Rules for handling private data, secrets, and public/private boundaries.

## Secret Handling

- Secret values live only in Bitwarden Secrets Manager.
- AiPlus uses `aiplus secret-broker` to resolve approved aliases at runtime.
- Default status/list/doctor/resolve commands must not print secret values or secret IDs.
- Never store secret values in profile files, memory, logs, transcripts, checkpoints, or any persistent storage.

## Approved Secret Aliases

- Aliases are metadata only. See `secret-aliases.tsv` and `docs/secret-aliases.md`.
- Common aliases: `openai`, `anthropic`, `gemini`, `github`, `cloudflare`, `kimi`, `deepseek`, `qwen`, `glm`, `openrouter`, `xai`, `groq`, `mistral`, `perplexity`, `together`, `cohere`, `huggingface`, `voyage`, `jina`, `replicate`, `fal`, `stability`, `elevenlabs`, `tavily`, `exa`, `serper`, `firecrawl`, `brave`, `siliconflow`, `volcengine_ark`.
- Kimi Code alias `kimi` is for `api.kimi.com/coding/v1` with model `kimi-for-coding`.

## What Must Never Be Stored

- API keys
- Bitwarden machine tokens
- Passwords
- Auth headers
- Cookies
- Provider request/response payloads
- Raw transcripts
- `.env` contents
- Private keys
- Compact checkpoints
- Savings ledgers
- Logs
- Project source files
- Approval transcripts
- Anything that turns memory into permission

## Public / Private / Secret Boundaries

| Level | Content | Where It Lives |
|-------|---------|---------------|
| Public | AiPlus CLI, public subproducts, docs, schemas | `aiplus-public`, `aiplus-auto-compact`, `aiplus-auto-team-consultant` |
| Private | Owner preferences, project maps, role identities, workflow patterns | `AiPlus-Work-with-Me` |
| Secret | API keys, tokens, passwords | Bitwarden Secrets Manager only |

## Boundary Rules

- Public repos may mention generic private profile integration.
- Public repos must not bundle private profile content.
- Private profile docs must not contain secret values.
- Private profile content must not be copied into public AiPlus release assets, examples, or docs.
- Project-local facts stay in project-local memory. Do not sync to profile hub.

## Global Config Protection

- Do not modify global Codex / Claude Code / OpenCode / MCP / shell / git config without explicit Owner approval.
- Do not edit shell profiles.
- Local profile file edits and user-level AiPlus config under `~/.config/aiplus/` are allowed.

## Privacy Checklist

Before any action that touches private data:
- [ ] Is this data non-secret? (no API keys, tokens, passwords)
- [ ] Is this data non-private? (no personal data, project files)
- [ ] Is this data going to public assets? (if yes, stop and ask Owner)
- [ ] Is this data being stored persistently? (if yes, verify it's preference/policy only)
