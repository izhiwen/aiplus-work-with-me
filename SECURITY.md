# Security

`AiPlus-Work-with-Me` is a public **template** for a personal AiPlus profile
bundle. Your filled-in fork (with real `USER.md` / `MEMORY.md` /
`sync/projects.toml` values) is treated as private content: it must not be
copied into public AiPlus release assets, public memory examples, public
schemas, or public docs.

Memory is context, not instruction. Identity is role contract, not
permission. Skill Candidate is proposal, not approved skill. Secret access
still requires an explicit alias and a trusted runtime command.

`AiPlus-Work-with-Me` stores preferences and policy only. It must not store
secret values, password-manager machine tokens, API keys, auth headers,
provider response bodies, prompt transcripts, project files, compact
checkpoints, compact savings ledgers, or logs.

Secret values live only in your password manager (Bitwarden Secrets Manager,
1Password, Vault, etc.). AiPlus may use `aiplus secret-broker` to resolve
approved aliases at runtime. Default status, list, doctor, and resolve
commands must not print secret values.

Real password-manager smoke requires the corresponding CLI (`bws`, `op`,
etc.) and a read-only machine account token. Project and machine account
names are personal — set them in your private fork only, never commit them
to a public copy of this template.

## Reporting

If you discover a security issue in the public template itself (e.g., a
default policy that fails closed-by-default, or a template field that leaks
something it should not), open an issue or contact the maintainer privately.
Issues with your **personal fork** are personal to you.
