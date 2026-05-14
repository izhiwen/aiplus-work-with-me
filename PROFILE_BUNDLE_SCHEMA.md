# Profile Bundle v2.1 Schema Contract

Schema version: 0.2.0
Status: DRAFT for Platform CEO review

## Overview

This document defines the schema contract for a complete private profile bundle in the AiPlus ecosystem. A profile bundle is a directory containing Owner preferences, role identities, memory policy, and sync rules. It is local/private by default and must never leak into public release assets.

## Required Files

### Core Files (Must Exist)

| File | Format | Purpose | CLI Check |
|------|--------|---------|-----------|
| `profile.toml` | TOML | Profile metadata, version, owner, structure map, boundaries | `profile.toml exists`, `profile.toml parseable`, `has name`, `has version`, `has owner` |
| `AGENTS.profile.md` | Markdown | Profile hub documentation, load order, natural language triggers | `AGENTS.profile.md exists`, `has heading` |

### Supplemental Files (Recommended)

| File | Format | Purpose | CLI Check |
|------|--------|---------|-----------|
| `USER.md` | Markdown | Stable Owner profile and collaboration preferences | `USER.md present`, `non-empty`, `redaction clean` |
| `MEMORY.md` | Markdown | Memory taxonomy, sync/forget/supersede rules | `MEMORY.md present`, `non-empty`, `redaction clean` |
| `preferences/` | Directory | Topic-based preference docs | `preferences/ present`, `valid` |
| `identities/` | Directory | Role identity TOML files | `identities/ present`, each `.toml` `valid` |
| `sync/` | Directory | Cross-project sync policy and evidence | `sync/ present`, `has policy or docs` |

### Optional Files

| File | Format | Purpose |
|------|--------|---------|
| `README.md` | Markdown | Public-facing profile description |
| `README.zh-CN.md` | Markdown | Chinese profile description |
| `SECURITY.md` | Markdown | Security policy and boundaries |
| `CHANGELOG.md` | Markdown | Version history |
| `secret-aliases.tsv` | TSV | Approved secret alias metadata (no values) |
| `docs/` | Directory | Additional documentation |

## profile.toml Schema

### Required Top-Level Fields

```toml
name = "string"                    # Canonical profile name
compatibility_aliases = ["string"] # Legacy name aliases
version = "string"                 # Semantic version
owner = "string"                   # Owner name
secret_values = "never-store"      # Must be "never-store"
subproduct = "private"             # Must be "private" for private profiles
```

### Required Sections

```toml
[structure]
readme = ["README.md"]
agents_profile = "AGENTS.profile.md"
profile_toml = "profile.toml"
user_snapshot = "USER.md"          # Optional but recommended
memory_snapshot = "MEMORY.md"      # Optional but recommended
preferences_dir = "preferences/"   # Optional
identities_dir = "identities/"     # Optional
sync_dir = "sync/"                 # Optional
docs_dir = "docs/"                 # Optional
security = "SECURITY.md"           # Optional
changelog = "CHANGELOG.md"         # Optional
secret_aliases = "secret-aliases.tsv"  # Optional

[structure.preferences]
communication = "preferences/communication.md"
engineering = "preferences/engineering.md"
# ... other preference files

[structure.identities]
ceo = "identities/ceo.identity.toml"
advisor = "identities/advisor.identity.toml"
# ... other identity files

[structure.sync]
policy = "sync/policy.toml"
projects = "sync/projects.toml"
# ... other sync files

[priority]
current_owner_message = 1
project_rules = 2
project_local_memory = 3
user_profile = 4
aiplus_generic_guidance = 5
model_defaults = 6

[defaults]
owner_facing_language = "Chinese"
public_docs_language = "English-first"
communication_style = "concise, conclusion-first, evidence-based"
workflow_tier_default = "MEDIUM"

[owner_gates]
secret_values = "never print or persist"
global_agent_config = "ask before changing"
publication = "requires explicit Owner approval"
push = "requires explicit Owner approval"
tag = "requires explicit Owner approval"
release = "requires explicit Owner approval"
deploy = "requires explicit Owner approval"
global_config = "requires explicit Owner approval"

[boundaries]
public_private_secret = "strict"
profile_content_in_public_assets = "forbidden"
secret_values_in_profile = "forbidden"
raw_transcripts_in_profile = "forbidden"
compact_checkpoints_in_profile = "forbidden"
logs_in_profile = "forbidden"
```

## Identity TOML Schema

### Required Fields

```toml
id = "aiplus.{role}.default"       # Unique identity ID
role = "string"                     # Role name (CEO, Advisor, Builder, etc.)
version = "0.2.0"                   # Schema version
scope = "project"                   # Scope: project, user, global
activation = ["trigger1", "trigger2"]  # Activation phrases/contexts
output_contract = "string"          # Expected output format
owner_gates = ["publish", "deploy"]  # Dangerous actions this role guards
inherits = ["aiplus-work-with-me when linked and available"]  # Parent profiles
```

### Optional v2 Fields

```toml
role_contract = "string"            # Human-readable role description
scope_boundaries = ["string"]       # Scope limitations
current_responsibilities = ["string"]  # Active responsibilities
allowed_actions = ["string"]        # Permitted actions
forbidden_actions = ["string"]      # Prohibited actions
memory_retrieval_policy = "string"  # What memory to load
owner_gate_policy = "string"        # How to handle Owner gates
result_packet_expectations = ["string"]  # Expected result packet fields
```

### CLI Validation

The CLI checks each identity file for:
- `valid`: File is non-empty and contains `name =` and `role =` fields
- `has owner gate`: File contains `owner_gate` or `ownerGate` text

### CLI-Compatible vs Human-Readable

- **`.aiplus/identities/`**: CLI-compatible TOML matching `RoleIdentity` struct
- **`identities/`**: Human-readable TOML with rich sections (`[contract]`, `[behavior]`, `[forbidden]`)

Both should coexist. The CLI reads from `.aiplus/identities/`. Humans read from `identities/`.

## Sync Policy Schema

### Required Structure

The `sync/` directory must contain at least one of:
- `policy.toml` (parseable TOML)
- `README.md` (markdown documentation)

### policy.toml Fields

```toml
[automatic_sync]
allow_when = ["string"]
candidate_creation_when = ["string"]
keep_local_when = ["string"]
forbidden = ["string"]

[priority]
1 = "Current Owner message"
2 = "Project AGENTS.md / project rules"
# ... etc

[conflict_resolution]
specific_beats_general = true
newer_explicit_beats_older = true
high_risk_becomes_blocked = true

[taxonomy]
global_preference = { sync = "profile_hub", auto_sync = true }
project_local = { sync = "project_local_only", auto_sync = false }
# ... etc
```

## Redaction Rules

Files `USER.md` and `MEMORY.md` must pass redaction checks:
- No lines containing: `api_key`, `apikey`, `api-key`, `secret_key`, `secret-key`, `access_token`, `access-token`, `password`, `private_key`, `bearer `, `authorization: `, `cookie:`, `-----begin `
- Exception: Lines starting with `[REDACTED]` are allowed
- Policy documentation should avoid exact trigger words or use `[REDACTED]` prefix

## Public/Private Boundary

### Forbidden in Public Assets

- `USER.md` content
- `MEMORY.md` content
- `preferences/` content
- `identities/` content (human-readable versions)
- `sync/` content (except generic policy references)
- `secret-aliases.tsv` content
- Any file containing `aiplus-work-with-me` specific preferences

### Allowed in Public Assets

- Generic references: "Private profiles such as `aiplus-work-with-me` may consume this engine"
- Template `inherits = ["aiplus-work-with-me when linked and available"]`
- `forbidden_files: [aiplus-work-with-me private content, any writes]`
- Module contract stating public assets must not include private profile

## Validation Contract for profile doctor

### Required Checks (All Must Pass)

| Check | Evidence |
|-------|----------|
| `profile.toml exists` | File exists in profile root |
| `profile.toml parseable` | Valid TOML syntax |
| `profile.toml has name` | `name` field present |
| `profile.toml has version` | `version` field present |
| `profile.toml has owner` | `owner` field present |
| `AGENTS.profile.md exists` | File exists |
| `AGENTS.profile.md has heading` | Contains markdown heading |
| `USER.md present` | File exists (if supplemental bundle) |
| `USER.md non-empty` | File has content |
| `USER.md redaction clean` | No unredacted secret patterns |
| `MEMORY.md present` | File exists (if supplemental bundle) |
| `MEMORY.md non-empty` | File has content |
| `MEMORY.md redaction clean` | No unredacted secret patterns |
| `preferences/ valid` | Directory exists and contains structured .md files |
| `identities/ valid` | Directory exists and all .toml files are valid |
| `sync/ has policy or docs` | Contains policy.toml or README.md |
| `sync/policy.toml parseable` | Valid TOML syntax (if policy.toml exists) |
| `private content not in public assets` | Manual review required |

### Recommended Checks

| Check | Evidence |
|-------|----------|
| `secret_values=none` | No secrets detected |
| `global_agent_config_edits=none` | No global config changes |
| `identity context --role {role}` | Returns valid identity context |
| `profile status` | Shows installed=yes |

## Template Self-Check (run on your fork)

| Requirement | Expected | How to check |
|-------------|----------|--------------|
| `profile.toml` | All required fields present, `owner` filled in | `cat profile.toml \| head` |
| `AGENTS.profile.md` | Has heading, documents structure | manual review |
| `USER.md` | Present, placeholders replaced with your values | manual review |
| `MEMORY.md` | Present, redaction clean | grep for forbidden tokens |
| `preferences/` | 5 topic files, all non-empty | `ls preferences/` |
| `identities/` | 7 role files, all valid, all have owner gates | `aiplus identity list` |
| `sync/` | `policy.toml` parseable, all 6 files present | `aiplus profile doctor` |
| Private boundary | No filled-in personal content in any public branch | manual review before push |
| Secret boundary | No secret values in profile files | `rg "sk-\|BEGIN .*PRIVATE KEY\|Authorization:\|Bearer "` |
| Global config | Untouched by template install | `aiplus doctor` |

## Profile Install Expectations

### Current Behavior (v0.5.1)

`aiplus profile install` copies to `~/.config/aiplus/profiles/{name}/`:
- `AGENTS.profile.md`
- `profile.toml`

Does NOT automatically copy:
- `USER.md`
- `MEMORY.md`
- `preferences/`
- `identities/`
- `sync/`
- `.aiplus/` (including `.aiplus/identities/`)

### Expected Full Bundle Install

Future CLI should copy all supplemental files:
- Core: `AGENTS.profile.md`, `profile.toml`
- Supplemental: `USER.md`, `MEMORY.md`
- Directories: `preferences/`, `identities/`, `sync/`
- CLI integration: `.aiplus/` (manifest, identities, refresh prompt)

### Drift Detection

After install, compare source vs installed:
```bash
# Check file presence
diff <(ls source/ | sort) <(ls installed/ | sort)

# Check identity files
diff source/identities/ installed/identities/
diff source/.aiplus/identities/ installed/.aiplus/identities/ 2>/dev/null || echo "MISSING"
```

## Backup and Recovery

### Backup Strategy

1. **Source repo backup**: The profile source in `aiplus-work-with-me/` is the source of truth. Keep it in version control.
2. **Installed profile backup**: `~/.config/aiplus/profiles/aiplus-work-with-me/` can be backed up as a tarball.
3. **Before reinstall**: Save existing installed profile to prevent data loss.

### Recovery Steps

1. **Corrupted profile.toml**: Restore from source repo or backup.
2. **Missing identity files**: Re-run `aiplus profile install` or manually copy from source.
3. **Missing .aiplus/identities/**: Copy from source repo `.aiplus/identities/` to installed profile.
4. **Sync policy errors**: Restore `sync/policy.toml` from source or backup.
5. **Session opt-out**: Say "本次忽略我的偏好" to disable profile for current session without modifying files.

### Rollback

```bash
# Disable profile for session
aiplus profile disable aiplus-work-with-me --user --yes

# Re-enable
aiplus profile install aiplus-work-with-me --user --yes

# Full uninstall
aiplus profile uninstall aiplus-work-with-me --user --yes
```

## Versioning and Migration

### Schema Versioning

- `profile.toml` `version` field tracks profile bundle version
- Identity files use `version = "0.2.0"` for CLI compatibility
- Schema changes should update version and CHANGELOG.md

### Migration Notes

- v0.2.x → v0.3.0: Added profile hub structure (USER.md, MEMORY.md, preferences/, identities/, sync/)
- v0.3.0 → v0.3.1: Added PROFILE_BUNDLE_SCHEMA.md, CLI-compatible identities, proper TOML policy
- Future: Consider unifying identity dual-track or adding full supplemental install sync

## Gaps and Recommendations

1. **CLI install sync**: `aiplus profile install` copies only `AGENTS.profile.md` and `profile.toml`. Full supplemental sync not implemented.
2. **Identity schema dualism**: Two versions of identity files (human-readable vs CLI-compatible). Installed profile lacks `.aiplus/identities/`.
3. **policy.toml redaction**: Future policy docs should avoid redaction trigger words in TOML values.
4. **MEMORY.md redaction sensitivity**: Strict redaction check requires careful wording.

## Next Steps

1. Platform CEO review of this schema contract
2. Decide if `aiplus profile install` should sync full bundle
3. Decide if identity dualism should be unified
4. Add `.aiplus/` to install scope
5. Update CLI `profile doctor` if schema changes approved
