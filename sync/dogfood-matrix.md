# Dogfood Matrix (TEMPLATE)

Local validation matrix to run after you personalize your fork and install
the profile. Each row is a check the agent or you should perform; fill in
**Status** and **Evidence** as you go.

> The Project column lists your own registered projects from
> `sync/projects.toml`. Add as many rows per check as you have projects.

## Projects (replace with your own)

- `[Project A]` (`~/Projects/[Project A]`)
- `[Project B]` (`~/Projects/[Project B]`)

## Validation Checklist

### 1. Profile Discovery / Install

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | Profile can be discovered or installed | _PENDING_ | `aiplus profile status` should show `profiles=[aiplus-work-with-you]`, installed=yes |
| `[Project B]` | Profile hub is discoverable | _PENDING_ | Project has (or can have) `.aiplus/memory/` for local memory integration |

### 2. USER / MEMORY Context Load

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | `USER.md` and `MEMORY.md` can be loaded | _PENDING_ | Files readable; `aiplus status` shows profile loaded |
| `[Project B]` | Can load global preferences | _PENDING_ | `sync/projects.toml` scope is `global-preferences-only` and no profile content leaks |

### 3. CEO Identity Dry-Run

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | CEO identity is coherent | _PENDING_ | `aiplus identity context --role ceo` reads `role_name=CEO Orchestrator` |
| `[Project B]` | CEO identity does not conflict | _PENDING_ | No project-local CEO rules conflict with the profile hub identity |

### 4. Advisor Identity Dry-Run

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | Advisor identity is coherent | _PENDING_ | `aiplus identity list` shows advisor=present |
| `[Project B]` | Advisor identity does not conflict | _PENDING_ | No project-local advisor rules conflict |

### 5. Synthetic Global Preference

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | Can represent a synthetic global preference | _PENDING_ | Example: "Use LIGHT for doc fixes" can be stored as GLOBAL_PREFERENCE |
| `[Project B]` | Can receive synthetic global preference | _PENDING_ | `sync/policy.toml` allows automatic sync for stable, non-secret, reversible preferences |

### 6. Synthetic Project-Local Fact Isolation

| Project | Check | Status | Evidence |
|---------|-------|--------|----------|
| `[Project A]` | Project-local fact does not leak | _PENDING_ | A project fact stays in `.aiplus/memory/` only; `promotion-rules.md` blocks auto-promotion |
| `[Project B]` | Project-local fact does not leak | _PENDING_ | Local-only; no content synced to profile hub |

### 7. Opt-Out Behavior

| Scope | Check | Status | Evidence |
|-------|-------|--------|----------|
| All | Opt-out is documented | _PENDING_ | `USER.md` and `preferences/communication.md` document opt-out phrases |
| All | Opt-out is session-local only | _PENDING_ | Documented: "Opt-out is session-local only. Next session reverts to normal profile loading." |

### 8. Secret Values Not Printed

| Scope | Check | Status | Evidence |
|-------|-------|--------|----------|
| All | Secret values are not in profile files | _PENDING_ | Run `rg "sk-\|BEGIN .*PRIVATE KEY\|Authorization:\|Bearer "` on the profile dir. Only policy references should match. |
| All | Secret values are not in memory docs | _PENDING_ | `MEMORY.md` and `preferences/` contain only policy and taxonomy |
| All | Secret broker does not print values | _PENDING_ | `aiplus secret-broker status` reports `secret_values_printed=no` |

### 9. Global Config Untouched

| Scope | Check | Status | Evidence |
|-------|-------|--------|----------|
| All | Global agent config unchanged | _PENDING_ | `~/.claude/`, `~/.codex/`, `~/.config/opencode/` not modified |
| All | Shell profiles unchanged unless explicitly approved | _PENDING_ | `~/.zshrc` / `~/.bashrc` / `config.fish` not modified without explicit consent |
| All | Git config unchanged | _PENDING_ | `git config --global` not modified; `aiplus doctor` confirms |

### 10. Public / Private Boundary

| Scope | Check | Status | Evidence |
|-------|-------|--------|----------|
| All | Public AiPlus assets do not contain your private profile content | _PENDING_ | Scan `aiplus-public`, `aiplus-auto-compact`, `aiplus-auto-team-consultant` for your filled-in placeholders (name, paths, namespace). Only generic template references should match. |

### 11. Doctor Status

| Scope | Check | Status | Evidence |
|-------|-------|--------|----------|
| Profile | `aiplus doctor` | _PENDING_ | `DOCTOR_STATUS=PASS` |
| Profile | `aiplus profile doctor` | _PENDING_ | `PROFILE_DOCTOR_STATUS=PASS` |
| Profile | `aiplus status` | _PENDING_ | `STATUS=PASS`, installed=yes, all identities present |

## Reference commands

```bash
aiplus profile status
aiplus profile doctor
aiplus status
aiplus doctor
aiplus memory status
aiplus identity list
aiplus identity context --role ceo
aiplus identity context --role advisor
aiplus secret-broker status

# Local scans
rg -n "sk-|BEGIN .*PRIVATE KEY|Authorization:|Bearer |password|token|api[_-]?key" .
rg -n "compact/checkpoints|current-handoff|provider response|raw transcript|\.env" .
```

## Notes

- Use synthetic, non-secret, non-private examples only.
- Do not edit production project code during validation.
- Local validation only — do not push results to a public branch.
- `aiplus profile install` copies only `AGENTS.profile.md` and `profile.toml`
  to `~/.config/aiplus/profiles/<name>/`. `USER.md`, `MEMORY.md`,
  `preferences/`, `identities/`, `sync/` remain in the source repository and
  are accessible via the filesystem.
- `.aiplus/` is gitignored in this repository because it contains
  AiPlus-managed runtime files.
