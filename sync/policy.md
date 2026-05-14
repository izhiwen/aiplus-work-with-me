# Sync Policy

Cross-project sync rules and priority for the `aiplus-work-with-you`
profile hub.

## Automatic Sync Rules

### Allow Automatic Sync When

- Owner explicitly says "以后所有项目", "以后都", "默认", "我的通用偏好", "所有 CEO", "所有 Advisor", or similar universal preference statements.
- The preference is stable, non-secret, non-project-specific, and reversible.
- The memory affects style, workflow, or context only.
- It does not authorize dangerous actions (push, release, deploy, secret access, global config).

### Allow Automatic Candidate Creation When

- The same preference appears in two or more projects.
- Owner corrects the same behavior repeatedly.
- A project pattern looks reusable but needs abstraction.

### Keep Local Only When

- It is a project fact (specific path, implementation detail, local bug, private customer data).
- It names a project-specific decision.
- It would confuse other projects if generalized.

### Forbidden Sync

- push/release/deploy approvals
- secret access permission
- external account permission
- global config permission
- raw logs/transcripts/checkpoints/provider payloads
- private project files

## Sync Priority

```
1. Current Owner message
2. Project AGENTS.md / project rules
3. Project-local .aiplus/memory
4. aiplus-work-with-you profile hub
5. Generic AiPlus guidance
6. Model defaults
```

## Conflict Resolution

- More specific beats more general.
- Newer explicit Owner correction beats older preference.
- High-risk conflicts become BLOCKED.
- Low-risk conflicts may use the highest-priority record and log the conflict.
- Every promoted memory must be explainable and reversible.

## Taxonomy Mapping

| Category | Sync Direction | Auto-Sync Allowed | Example |
|----------|---------------|-------------------|---------|
| GLOBAL_PREFERENCE | Profile hub | Yes, if stable and non-secret | "所有项目都用 rtk 前缀" |
| GLOBAL_WORKFLOW | Profile hub | Yes, if stable and non-secret | "长任务前先 compact" |
| GLOBAL_TOOLING_HINT | Profile hub | Yes, if stable and non-secret | "默认用 Kimi Code" |
| PROFILE_CONTEXT | Profile hub only | Yes, private only | "Owner 的语言偏好" |
| PROJECT_PATTERN | Profile hub (candidate) | Yes, after abstraction | "两个项目都遇到同个问题" |
| PROJECT_LOCAL | Project-local only | No | "AppModules 的模块路径" |
| ROLE_CONTRACT | Profile hub `identities/` | Yes, role behavior only | "CEO 应拆分任务" |
| SKILL_CANDIDATE | Profile hub evidence-log | No, candidate only | "建议做成 skill" |
| DENY_MEMORY | Nowhere | No | API keys, passwords |

## Reversibility

Every synced preference must be reversible:
- Owner can say "忘掉这个" to remove.
- Owner can say "恢复之前的偏好" to rollback.
- Superseded preferences are kept for audit trail but marked inactive.
