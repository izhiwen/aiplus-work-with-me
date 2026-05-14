# Engineering Preferences

Stable Owner preferences for engineering work, code style, and technical decisions.

## General Principles

- Evidence over assumption. Distinguish "I verified" from "I assume".
- Local-first: prefer local edits, local tests, local scans before any remote action.
- Atomic changes: one concern per commit, one issue per fix.
- File claims: respect claimed files; do not edit files claimed by another worker without handoff.

## Code Style

- Follow existing conventions in the codebase.
- Never assume a library is available without checking `package.json`, `Cargo.toml`, or neighboring files.
- Look at existing components/patterns before creating new ones.
- Security best practices: never introduce code that exposes or logs secrets.

## Rust (AiPlus Ecosystem)

- Follow existing Rust patterns in `aiplus-public`.
- CLI/core boundary clarity: CLI is thin, core is testable.
- Release readiness: version bump, CHANGELOG update, tests pass before release consideration.
- Do not publish or release without Owner approval.

## TypeScript / JavaScript (PAL, AppModules)

- Follow existing patterns in the target project.
- Check `tsconfig.json`, `package.json`, and existing source before adding dependencies.
- Prefer existing utilities and helpers.

## Testing

- Verify with tests when possible.
- Never assume test framework; check README or search codebase first.
- Run lint and typecheck after code changes if commands are known.

## Git

- Use `rtk git status`, `rtk git diff` in AiPlus workspace.
- Do not push without explicit Owner approval.
- Do not create tags or releases without explicit Owner approval.
- Do not force-push to main/master.
- Commit messages: concise, focus on "why" not "what".

## Documentation

- Update docs when behavior changes.
- Keep CHANGELOG, README, and AGENTS.md in sync.
- Do not create documentation files unless explicitly requested or behavior changed.
