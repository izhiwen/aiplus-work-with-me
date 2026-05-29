# MEMORY.md

What should be remembered, synced, forgotten, superseded, or rejected in
your `AiPlus-Work-with-Me` profile hub.

## Memory Taxonomy

Use these exact categories:

| Category | Description | Sync Scope | Example |
|----------|-------------|------------|---------|
| GLOBAL_PREFERENCE | Stable Owner preference across all projects | Profile hub | "以后所有项目都用 LIGHT 做文档修复" |
| GLOBAL_WORKFLOW | Recurring workflow preference across projects | Profile hub | "长任务前先 compact" |
| GLOBAL_TOOLING_HINT | Preferred tools, command style, model/runtime habits | Profile hub | "在 AiPlus workspace 用 rtk 前缀" |
| PROFILE_CONTEXT | Personal Owner profile context (private to you) | Profile hub only | 用户语言偏好、项目地图 |
| PROJECT_PATTERN | Reusable lesson abstracted from one project | Profile hub (candidate) | "两个项目都遇到同样问题，抽象成通用模式" |
| PROJECT_LOCAL | Fact specific to one project; must not sync globally | Project-local `.aiplus/memory/` | "AppModules 的某某模块路径是 X" |
| SKILL_CANDIDATE | Proposed skill only; not approved skill | Profile hub `sync/evidence-log.md` | "建议把某某流程做成 skill" |
| DENY_MEMORY | Content that must never be stored or synced | Nowhere | API keys、密码、原始 transcript |

## Remember Rules

1. **Owner explicit universal preference**: When Owner says "以后所有项目"、"以后都"、"默认"、"我的通用偏好"、"所有 CEO"、"所有 Advisor" — create or update GLOBAL_PREFERENCE.
2. **Repeated correction**: When Owner corrects the same behavior in two or more projects — create PROJECT_PATTERN candidate, then promote to GLOBAL_PREFERENCE after abstraction.
3. **Project fact**: Store in project-local `.aiplus/memory/` only. Do not sync to profile hub.
4. **Skill proposal**: Log as SKILL_CANDIDATE in `sync/evidence-log.md`. Do not auto-create skill files.

## Sync Rules

- Automatic sync allowed for: stable, non-secret, non-project-specific, reversible preferences that affect style/workflow/context only.
- Automatic sync blocked for: permissions (push/release/deploy), secrets, global config, external accounts, raw logs/transcripts/checkpoints.
- Project-local facts must stay in project-local memory. They must not leak into profile hub.
- PROFILE_CONTEXT stays in profile hub only. It must not leak into public AiPlus assets.

## Forget / Supersede / Reject

| Action | Trigger | Behavior |
|--------|---------|----------|
| Forget | Owner says "忘掉这个" | Remove by memory ID; if ambiguous, ask which ID |
| Supersede | Newer explicit Owner correction contradicts old preference | Mark old as superseded; new preference takes effect; keep old for audit trail |
| Reject | Owner says "不要记这个" or content is DENY_MEMORY | Do not store; log rejection reason in evidence-log |
| Stale | Preference unused for 6+ months and no project references it | Mark stale; do not auto-delete; flag in evidence-log |
| Rollback | Owner says "恢复之前的偏好" | Re-activate the most recent non-superseded version; log rollback |

## Never Store as Memory

- API keys
- Bitwarden machine tokens
- Passcodes
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

## Conflict Priority

1. Current Owner message
2. Project AGENTS.md / project rules
3. Project-local `.aiplus/memory`
4. `AiPlus-Work-with-Me` profile hub
5. Generic AiPlus guidance
6. Model defaults

## Evidence Requirements

Every promoted memory (project-local -> profile hub) must be:
- Explainable: why it was promoted
- Reversible: how to undo it
- Non-secret: no secret values
- Non-project-specific: abstracted from concrete project facts
