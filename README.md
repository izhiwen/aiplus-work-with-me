# AiPlus work-with-you

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[中文 README](README.zh-CN.md)

A portable user-profile bundle for AiPlus. Solves cross-project, cross-session
amnesia: your agent remembers _you_ — your collaboration style, your project
map, your role identities, your tooling preferences — without you re-stating
them in every new session or every new repo.

## The pain

On Monday you tell your agent in project A:

> "Reply in Chinese. Keep responses conclusion-first. Don't push without
> asking. We use LIGHT / MEDIUM / HEAVY tiers."

On Tuesday you open project B and start a new session. The agent has no idea
who you are. It writes verbose English summaries, suggests pushing to main,
and asks what "MEDIUM tier" means.

The fix is not "tell the agent everything again." The fix is a structured,
portable profile that lives outside any one project, that any AiPlus-aware
agent can load when you cd into a project — and that you control.

## What this is

`AiPlus-Work-with-Me` is a **template repository** for that profile bundle.
You fork it, fill in the placeholders, install it once with `aiplus profile
install`, and every AiPlus-aware agent in every project on your machine
inherits the same understanding of how you work.

It carries five kinds of context:

- **`USER.md`** — your stable Owner profile: name, primary workspace,
  language, decision authority, dangerous-action gates.
- **`preferences/`** — five topic-sliced preference docs: communication
  style, engineering taste, CEO/review workflow, tools and runtimes, privacy
  and secrets.
- **`identities/`** — seven role identity contracts (CEO, Advisor, Rust
  Lead, Runtime QA, Security Reviewer, Reviewer, Builder). Reusable role
  scaffolding for orchestrated agents.
- **`sync/`** — cross-project sync rules, project registry, promotion rules,
  conflict/staleness handling. Decides what is universal vs. project-local.
- **`MEMORY.md`** — memory taxonomy and the remember / sync / forget /
  supersede / reject rules that govern how preferences accumulate over time.

It deliberately carries **no secrets**: no API keys, no Bitwarden tokens, no
provider payloads, no transcripts, no project files, no compact checkpoints.
Aliases and policy only. See [SECURITY.md](SECURITY.md).

## Install

```bash
# 1. Fork or clone this repo somewhere local (not inside a project tree)
git clone https://github.com/<your-account>/AiPlus-Work-with-Me.git
cd AiPlus-Work-with-Me

# 2. Personalize the placeholders
#    USER.md            — name, workspace, language, gates
#    profile.toml       — owner field, defaults
#    sync/projects.toml — your local project paths and scopes
#    docs/project-map.md
#    secret-aliases.tsv — your Bitwarden alias namespace

# 3. Install as a user-level AiPlus profile
aiplus profile install AiPlus-Work-with-Me --user --yes
```

Verify:

```bash
aiplus profile status
aiplus profile doctor
aiplus refresh
aiplus status
aiplus doctor
```

## Agent Continuity natural-language entry points

When an AiPlus-aware agent loads this profile, the following phrasings (in
either Chinese or English) map to memory operations:

| You say | Agent does |
| --- | --- |
| 记住这个 / "remember this" | Redacted project-local memory write |
| 记住这个偏好 / "remember this preference" | Profile-level GLOBAL_PREFERENCE candidate |
| 以后都这样 / "from now on" | Profile/global candidate; do not silently approve |
| 只在这个项目用 / "only in this project" | Project-local memory only |
| 忘掉这个 / "forget this" | Forget by memory id; ask if ambiguous |
| 你记住了什么 / "what do you remember" | Memory status |
| 这次用了哪些记忆 / "which memories were used" | Memory context dump |
| 新开顾问 / "new advisor" | Load advisor identity context |
| 新开 CEO / "new CEO" | Load CEO identity context |
| 把这次经验沉淀成 skill / "make this a skill" | Skill Candidate proposal only |
| 不要用我的私人记忆 / 本次忽略我的偏好 | Session-local opt-out |

## Boundaries — what this is NOT

This profile **describes** preferences. It does **not** grant permission.

Memory is context, not instruction. Identity is a role contract, not a
permission grant. Skill Candidates are proposals, not approved skills. The
profile **cannot** authorize:

- push, publish, release, deploy
- secret access or secret exposure
- edits to global agent or shell config
- changes to external accounts

Every dangerous action still requires an explicit Owner message in the live
session. See [docs/owner-gates.md](docs/owner-gates.md).

## Relationship to other AiPlus modules

`AiPlus-Work-with-Me` is the user-profile bundle layer. It composes with:

- **`aiplus-agent-memory`** — the project-local JSONL memory engine. This
  profile sets the taxonomy and sync rules; agent-memory stores the actual
  records in `.aiplus/memory/` inside each project.
- **`aiplus-secret-broker`** — runtime-only secret resolution through your
  password manager. This profile lists alias names only; the broker resolves
  values from Bitwarden / etc. at run time.
- **`aiplus-auto-compact`** — compact / handoff workflows.
- **`aiplus-auto-team-consultant`** — multi-agent consultation workflows.

## Forking workflow

This repo is intended to be **forked, personalized, and made private** —
your filled-in `USER.md`, `MEMORY.md`, `preferences/`, and
`sync/projects.toml` may contain personal context you do not want public.

Recommended pattern:

```bash
# Fork on GitHub (public template) → clone your private fork (or rename it
# aiplus-work-with-<your-name> and flip visibility to private)
gh repo fork izhiwen/AiPlus-Work-with-Me --clone --fork-name aiplus-work-with-<your-name>
gh repo edit izhiwen/aiplus-work-with-<your-name> --visibility private
```

Or just download a tarball if you would rather not have a fork relationship.

## License

Apache-2.0. See [LICENSE](LICENSE).
