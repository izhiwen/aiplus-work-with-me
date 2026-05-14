# Evidence Log

Append one entry per dogfood / scan / validation cycle on your personal
fork. Each entry should be reproducible: anyone re-running the same commands
on the same checkout should land on the same evidence.

The template ships **empty on purpose** — the example entry below is a
placeholder showing the schema. Delete it when you start logging real runs.

---

## Entry schema

```
- Date: YYYY-MM-DD
- Cycle: <short tag, e.g. "initial-install", "post-fork", "dogfood-2026Q2">
- Description: <one-paragraph summary>
- Commands run:
    <verbatim shell commands, one per line>
- Evidence: <observed outputs, file diffs, or scan summaries>
- Verdict: PASS | NEEDS_FIX | BLOCKED
- Follow-up: <next action, or "none">
```

## Example entry (delete after first real run)

- Date: YYYY-MM-DD
- Cycle: initial-install
- Description: First install of personal `aiplus-work-with-me` fork after
  filling in `USER.md`, `sync/projects.toml`, and `secret-aliases.tsv`.
- Commands run:
    aiplus profile install aiplus-work-with-me --user --yes
    aiplus profile status
    aiplus profile doctor
    aiplus doctor
- Evidence: `profile_doctor` returned `PASS` for all checks; `profile
  status` reported `installed=yes`; no global config was modified.
- Verdict: PASS
- Follow-up: none
