# Promotion Rules

Project memory -> profile memory promotion rules.

## When to Promote

1. **Owner explicitly requests universal preference**
   - Trigger: "以后所有项目", "以后都", "默认", "我的通用偏好"
   - Action: Create or update GLOBAL_PREFERENCE in profile hub.
   - Evidence: Log Owner message in `sync/evidence-log.md`.

2. **Repeated pattern across two or more projects**
   - Trigger: Same preference or correction appears in 2+ projects.
   - Action: Create PROJECT_PATTERN candidate in profile hub. After abstraction and Owner confirmation (or implicit acceptance), promote to GLOBAL_PREFERENCE.
   - Evidence: Log projects involved and the pattern.

## Promotion Process

```
Project-local memory
       |
       v
   [Candidate] --(abstraction)--> PROJECT_PATTERN
       |
       v
   [Verification] --(stable, non-secret, reversible)--> GLOBAL_PREFERENCE
       |
       v
   [Log in evidence-log.md]
```

## Auto-Promotion Criteria

A project-local memory MAY be auto-promoted to profile hub when ALL are true:
- Owner has explicitly stated it as universal, OR it appears in 2+ projects with the same meaning.
- It is non-secret (no API keys, tokens, passwords).
- It is non-project-specific (abstracted from concrete paths/details).
- It is reversible (can be forgotten or superseded).
- It does not authorize dangerous actions.
- It affects style, workflow, or context only.

## Manual Promotion Required

A project-local memory MUST NOT be auto-promoted when ANY are true:
- It is a project fact (specific path, implementation detail, local bug).
- It is a preference that has only appeared in one project.
- Owner has not explicitly stated it as universal.
- It could confuse other projects if generalized.
- It relates to a specific product decision.

## Promotion Log

Every promotion must be logged in `sync/evidence-log.md` with:
- Source project(s)
- Original memory content (redacted if needed)
- Abstracted preference content
- Promotion reason
- Timestamp
- Reversibility note
