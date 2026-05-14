# Agent Workflows

Workflow tiers:

- LIGHT: narrow check, small wording/code fix, one command, one scan.
- MEDIUM: one feature cluster, focused review, docs sync, or targeted QA.
- HEAVY: release, security, multi-repo synchronization, private/public boundary,
  or high-risk work.

For large goals, use CEO orchestration:

- create task cards
- delegate research, implementation, docs, security, and QA where useful
- integrate results
- run final independent verification
- report PASS, NEEDS_FIX, or BLOCKED with evidence

Current Owner message and project rules always outrank this profile.

Agent Continuity natural-language mapping:

- `记住这个` / `记住这个偏好`: project memory after redaction.
- `以后都这样`: profile/global candidate only; no silent approval.
- `只在这个项目用`: project memory.
- `忘掉这个`: `aiplus memory forget <id>`, or ask which memory if ambiguous.
- `你记住了什么` / `这次用了哪些记忆`: `aiplus memory status` and context.
- `新开顾问` / `新开 advisor`: advisor Role Identity context.
- `新开 CEO`: CEO Role Identity context.
- `把这次经验沉淀成 skill`: Skill Candidate, not approved skill.
- `不要用我的私人记忆` / `本次忽略我的偏好`: session-local opt-out only.

Memory cannot authorize publish, push, deploy, secret use, external accounts, or
global config edits. Identity grants no permission.
