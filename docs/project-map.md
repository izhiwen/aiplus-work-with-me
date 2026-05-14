# Project Map

> Template — replace the example paths with your own. The categories and
> public/private boundary rules are the structural part you should keep.

Example AiPlus workspace (public modules; canonical names — do not rename):

- `~/Projects/AiPlus/aiplus-public` — public AiPlus Rust CLI and release source.
- `~/Projects/AiPlus/aiplus-auto-compact` — public Auto Compact subproduct.
- `~/Projects/AiPlus/aiplus-auto-team-consultant` — public Auto Team Consultant subproduct.
- `~/Projects/AiPlus/aiplus-agent-memory` — public Agent Memory engine.

Example personal projects (replace with your own):

- `~/Projects/<your-product>` — your active product workspace.
- `~/Projects/<your-experiments>` — experiments / sandbox / one-off scripts.

The full machine-readable registry lives in `sync/projects.toml`. Keep this
markdown version as the human-readable summary you skim when you forget what
lives where.

## Public / private boundary

- Public repos may mention generic private-profile integration.
- Public repos must not bundle your filled-in profile content.
- Your personal profile docs must not contain secret values.
