# Conflict and Staleness Rules

Conflict resolution and memory expiry rules.

## Conflict Priority

1. Current Owner message
2. Project AGENTS.md / project rules
3. Project-local `.aiplus/memory`
4. `AiPlus-Work-with-Me` profile hub
5. Generic AiPlus guidance
6. Model defaults

## Conflict Types

### Preference Conflict

- Two preferences in profile hub contradict each other.
- Resolution: Newer explicit Owner correction wins. If no explicit correction, the more specific wins.
- Log conflict in `sync/evidence-log.md`.

### Project-Local vs Profile Hub Conflict

- Project-local memory contradicts profile hub preference.
- Resolution: Project-local wins for project-specific facts; profile hub wins for universal preferences.
- If ambiguous, BLOCK and ask Owner.

## Conflict Behavior

| Risk Level | Behavior |
|------------|----------|
| High-risk | BLOCK and ask Owner |
| Medium-risk | Use highest-priority record; log conflict; flag for Owner review |
| Low-risk | Use highest-priority record; log conflict silently |

## Staleness Rules

### Stale Detection

A memory is considered stale when:
- It has not been referenced in any project for 6+ months.
- It has been superseded by a newer preference.
- The project it originated from no longer exists.
- Owner explicitly says "这个偏好好像过时了".

### Stale Handling

- Mark stale memory with `[STALE]` prefix or `stale = true` metadata.
- Do not auto-delete stale memories.
- Flag in `sync/evidence-log.md`.
- Owner may request deletion or revival.

### Revival

- Owner says "恢复这个偏好" or "这个还是有效的".
- Remove stale mark; re-activate.
- Log revival in `sync/evidence-log.md`.

## Supersession Rules

### When to Supersede

- Newer explicit Owner correction contradicts old preference.
- A better abstraction replaces a project-specific pattern.

### Supersession Process

1. Mark old preference as superseded with `superseded_by = "new-preference-id"`.
2. Create or update new preference.
3. Log supersession in `sync/evidence-log.md`.
4. Keep old preference for audit trail.

## Rollback Rules

### When to Rollback

- Owner says "恢复之前的偏好" or "回到上次的设置".
- A new preference causes unexpected issues.

### Rollback Process

1. Identify the most recent non-superseded version.
2. Re-activate it.
3. Mark the problematic preference as superseded.
4. Log rollback in `sync/evidence-log.md`.
