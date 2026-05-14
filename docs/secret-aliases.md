# Secret Aliases

Approved aliases are metadata only. Secret values must remain in your
password manager (Bitwarden Secrets Manager, 1Password, Vault, etc.).

## Template fields to fill in your private fork

- Password-manager project / vault name: `[your-secrets-project]`
- Machine account (read-only): `[your-machine-account]`
- Alias namespace prefix: `[your-namespace]` (e.g., your handle)

Use:

```bash
aiplus secret-broker list
aiplus secret-broker status
aiplus secret-broker doctor
```

Use `aiplus secret-broker run --aliases <a>,<b>,<c> -- <command...>` only for
explicit runtime secret needs with trusted commands. Prefer explicit aliases
so unused placeholder providers do not block unrelated commands. Do not
print, paste, log, summarize, compact, or persist secret values.

AiPlus resolves each alias key/name to a vault secret ID in memory before
fetching the value through the vault CLI. Default `resolve` output may say
`secret_id_found=yes`, but it must not print the secret ID or secret value.

## Common provider aliases shipped in `secret-aliases.tsv`

`openai`, `anthropic`, `gemini`, `github`, `cloudflare`, `openrouter`, `xai`,
`groq`, `mistral`, `perplexity`, `together`, `cohere`, `huggingface`,
`voyage`, `jina`, `replicate`, `fal`, `stability`, `elevenlabs`, `tavily`,
`exa`, `serper`, `firecrawl`, `brave`, `kimi`, `deepseek`, `qwen`, `glm`,
`siliconflow`, `volcengine_ark`.

These are **alias names**. The Bitwarden / vault path column in
`secret-aliases.tsv` is a placeholder you should rewrite to match your own
vault layout.
