# USER.md

Stable Owner profile and collaboration preferences.

> Template note: replace every `[Your …]` placeholder with your own value.
> Keep field names in English; values can be in whatever language you want
> the agent to mirror back to you.

## Identity

- Name: `[Your Name]`
- Role: `[Your Role(s) — e.g. product owner / engineer / investor]`
- Primary workspace: `~/your-workspace` (absolute path on your machine)
- Language: `[your preferred Owner-facing language]` for Owner-facing
  responses; English for schemas, commands, code

## Collaboration Style

- Concise, conclusion-first updates.
- Evidence-based decisions; distinguish assumption from verified fact.
- File paths, commands, schema keys, status enums, repo names, model names, and secret aliases in English.
- Hidden chain-of-thought should not be revealed in final responses.
- Progress updates for long work; final response includes verification and limitations.

## Decision Authority

- Owner has final say on all dangerous actions: push, publish, release, deploy, global config, secret exposure.
- Current Owner message outranks project rules; project rules outrank profile; profile outranks generic AiPlus guidance.
- Agents may make reasonable assumptions for low-risk local work.
- Agents must stop and ask for explicit approval for high-risk actions.

## Communication Preferences

- Default response language: `[your preferred language]`.
- Technical terms: English (file paths, commands, enums, repo names).
- Status reports: structured, with PASS/REVISE/BLOCKED verdicts.
- Avoid emojis unless explicitly requested.

## Project Boundaries

- Public AiPlus products this profile composes with (canonical names; do not
  rename): `aiplus-public`, `aiplus-auto-compact`,
  `aiplus-auto-team-consultant`, `aiplus-agent-memory`.
- Your project list lives in `sync/projects.toml`. Edit that file (not this
  one) to register your local projects and their profile scope.
- Profile content (your filled-in `USER.md`, `MEMORY.md`, `preferences/`,
  `sync/projects.toml`) must never be copied into a public AiPlus release
  asset.

## Session Opt-Out

- Owner may say: "本次忽略我的偏好", "关闭 work-with-you", "只看项目规则" (or
  the English equivalents: "ignore my profile this session", "disable
  work-with-you", "project rules only").
- Result: ignore this profile for current session; follow project rules and
  model defaults only.

## Stable Preferences (Non-Exhaustive — fill in your own)

Examples of the kind of universal preference that belongs here. Replace
with your own:

- Prefer local-only work for private profile changes.
- Prefer `[your preferred command prefix / wrapper]` in `[which workspace]`.
- Prefer LIGHT/MEDIUM/HEAVY workflow tier classification.
- Prefer CEO orchestration for large goals.
- Prefer Auto Compact before long-running work.
- Prefer Auto Team Consultant for advice and review.
- Prefer `[your password manager / secret store]` for secret values; never
  store secrets in profile files.

## Dangerous Actions (Always Require Explicit Approval)

- git push to remote
- Creating or moving tags
- Creating GitHub Releases
- Uploading artifacts
- Publishing to package registries
- Changing repository visibility
- Copying private profile content into public repos
- Modifying global Codex/Claude Code/OpenCode/MCP/shell/git config
- Editing shell profiles
- Deploying or changing external accounts
- Printing, storing, or transmitting secret values
- Uploading private logs, transcripts, checkpoints, or project files

## Memory Is Context, Not Permission

This USER.md describes preferences and boundaries. It does not grant permission to perform dangerous actions. Every dangerous action still requires an explicit Owner message or approval.
