# aiplus-work-with-me

Public template for an AiPlus user-level working profile. Fork it, fill in
the placeholders, install it once, and any AiPlus-aware agent on your
machine will read it.

## Load Order

1. Current Owner message
2. Project rules such as AGENTS.md, CLAUDE.md, or OpenCode project rules
3. Project-local `.aiplus/memory/`
4. `aiplus-work-with-me` profile hub
5. AiPlus generic guidance
6. Model defaults

## Profile Hub Structure

```
aiplus-work-with-me/
  README.md / README.zh-CN.md
  AGENTS.profile.md
  profile.toml
  USER.md              # Stable Owner profile and collaboration preferences
  MEMORY.md            # Remember/sync/forget/supersede/reject rules
  preferences/
    communication.md   # Communication style and natural-language triggers
    engineering.md     # Engineering preferences and code style
    ceo-and-review.md  # CEO orchestration and review workflow preferences
    tools-and-runtimes.md  # Preferred tools, CLI patterns, model habits
    privacy-and-secrets.md # Privacy boundaries and secret handling
  identities/
    ceo.identity.toml
    advisor.identity.toml
    rust-lead.identity.toml
    runtime-qa.identity.toml
    security-reviewer.identity.toml
    reviewer.identity.toml
    builder.identity.toml
  sync/
    policy.toml        # Cross-project sync rules and priority
    projects.toml      # Local project registry (TEMPLATE — fill in your projects)
    promotion-rules.md # Project memory -> profile memory promotion
    conflict-and-staleness.md # Conflict resolution and expiry rules
    dogfood-matrix.md  # Local validation matrix
    evidence-log.md    # Dogfood evidence and scan results
  docs/
    owner-gates.md
    project-map.md
    install-update-uninstall.md
    agent-workflows.md
    secret-aliases.md
    working-style.md
  PROFILE_BUNDLE_SCHEMA.md  # Profile bundle v2.1 schema contract
  SECURITY.md
  CHANGELOG.md
  secret-aliases.tsv
  .aiplus/             # AiPlus CLI integration (gitignored)
    manifest.json      # Profile-only stub manifest
    AGENTS.aiplus.md   # AiPlus refresh entry point
    REFRESH_PROMPT.txt # Refresh prompt
    identities/        # CLI-readable identity files
    memory/            # Local memory directory
```

## Default Owner-Facing Style

- Use the Owner's preferred language (see `USER.md → Identity → Language`)
  for progress and final summaries.
- Keep commands, paths, schema keys, status enums, repo names, model names,
  and secret aliases in English.
- Report decisions, evidence, blockers, and next actions only.

## Natural Language Mappings

- "我的偏好生效了吗", "aiplus-work-with-me status", "work-with-you status",
  or "is my profile installed":
  run `aiplus profile status`.
- "secret 状态", "看看 secret", "检查 API key", or "API key 是否可用":
  run `aiplus secret-broker status` or `aiplus secret-broker doctor`.
- To see approved provider aliases, run `aiplus secret-broker list`. The
  template's `secret-aliases.tsv` ships with common provider names —
  `openai`, `anthropic`, `gemini`, `github`, `cloudflare`, `openrouter`,
  `groq`, `mistral`, `perplexity`, `tavily`, `firecrawl`, etc. Replace the
  Bitwarden namespace column with your own.
- "update AiPlus", "升级 AiPlus", or "更新 AiPlus": run `aiplus update all`.
- "保存进度", "准备 compact", or "做个交接": run `aiplus compact prepare`.
- "继续", "resume", "refresh", or "刷新" after compact: run
  `aiplus compact resume` when appropriate.

## Secret Handling

- Use `aiplus secret-broker run --aliases <a>,<b>,<c> -- <command>` for
  explicit runtime secret needs. Prefer explicit aliases so unused
  placeholder providers do not block unrelated commands.
- Do not print, log, persist, compact, summarize, or paste secret values.
- Real Bitwarden / 1Password / Vault smoke checks require the corresponding
  CLI and a read-only machine account token. If unavailable, report that
  real-vault verification is unverified and use mock/local metadata checks
  only.
- If secret access is unavailable, report one exact fix command.

## Session-Local Opt Out

- If the Owner says "本次忽略我的偏好", "关闭 work-with-you", "只看项目规则",
  or the English equivalents, ignore this profile for the current session.

## Project Map

The project map is **personal** and lives in `sync/projects.toml` (TOML, for
machine consumption) and `docs/project-map.md` (Markdown, for human reading
and agent context). Both ship as templates — fill them in with your own
project paths and scopes. Example template values:

- `~/Projects/AiPlus` — AiPlus ecosystem workspace
- `~/Projects/<your-product>` — your active product workspace
- `~/Projects/<your-experiments>` — experiments / sandbox

## Workflow Defaults

- Use LIGHT for narrow checks or small edits.
- Use MEDIUM for one feature cluster, docs sync, or focused QA.
- Use HEAVY for release, security, multi-repo synchronization, or high-risk work.
- For large goals, act as CEO orchestrator: split tasks, delegate where useful,
  integrate, verify, and report with evidence.
- Do not call work complete without concrete QA, scans, and a final audit.

## Role Identity Quick Reference

| Role | File | Responsibility |
|------|------|---------------|
| CEO | `identities/ceo.identity.toml` | Goal setting, task decomposition, delegation, integration |
| Advisor | `identities/advisor.identity.toml` | Direction, tradeoffs, strategy, boundaries |
| Rust Lead | `identities/rust-lead.identity.toml` | Rust architecture, CLI/core boundary, release readiness |
| Runtime QA | `identities/runtime-qa.identity.toml` | Command dogfood, reproducible evidence, bug classification |
| Security Reviewer | `identities/security-reviewer.identity.toml` | Boundary checks, Owner gates, secret scan |
| Reviewer | `identities/reviewer.identity.toml` | Findings first, severity ordered, evidence verification |
| Builder | `identities/builder.identity.toml` | Scoped local implementation, file claim respect |

Identity is role contract, not permission.
