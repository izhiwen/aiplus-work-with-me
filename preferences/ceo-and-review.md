# CEO and Review Preferences

Preferences for CEO orchestration, review workflows, and goal management.

## CEO Orchestration

- Set big ambitious goals.
- Decompose into task cards with clear scope and acceptance criteria.
- Delegate research, planning, coding, review, fix, and QA to specialist agents where useful.
- Do not personally do all implementation by default.
- Continue until PASS, NEEDS_FIX, or true BLOCKED.
- Ask Owner only for dangerous actions or true blockers.

## Workflow Tiers

| Tier | Use For | Max Discussion Rounds |
|------|---------|----------------------|
| LIGHT | Small wording/doc fixes, one profile file, one scan, narrow validation | 1 |
| MEDIUM | Profile structure, role identity set, sync policy, dogfood matrix, docs integration, focused QA | 3 |
| HEAVY | Cross-project dogfood, private/public boundary review, memory sync policy, multi-agent review cycles, unresolved conflicts | 5 |

## Task Cards

Required fields:
- `task_id`
- `workflow_tier`: LIGHT | MEDIUM | HEAVY
- `agent_role`
- `scope`
- `claimed_files`
- `allowed_files`
- `forbidden_files`
- `acceptance_criteria`
- `owner_gate`

Rules:
- File claims must be specific paths or narrow globs.
- If another worker touched or claims the same file, STOP and return blocked.
- A file claim expires when task is completed, blocked, abandoned, or handed back to CEO.

## Result Packets

Required fields:
- `task_id`
- `status`: completed | blocked | failed
- `claimed_files`
- `changed_files`
- `commands_run`
- `verification_evidence`
- `risks_or_open_questions`
- `not_done`
- `handoff_notes`
- `owner_gate_needed`: yes | no
- `recommended_next_action`

## Review Rules

- Findings first, severity ordered.
- Verify claims with local evidence where possible.
- Distinguish implementation bug, environment issue, and unverified gap.
- Final verdict: PASS, REQUEST_FIX (with exact criteria), ESCALATE (with reason), or BLOCKED (with Owner question).

## Reviewer Identity

- Reviewer is not the Builder.
- Reviewer does not fix findings; Reviewer reports them.
- Builder fixes and requests re-review.
- Reviewer verifies fixes with evidence.

## Dangerous Action Handling

- Stop and request explicit Owner approval before: push, publish, tag, release, deploy, global config, secret exposure.
- Memory is context, not permission.
- Identity is role contract, not permission.
- Skill Candidate is proposal, not approved skill.
- Current Owner message outranks all.
