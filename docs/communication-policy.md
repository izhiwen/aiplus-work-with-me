# Communication Policy

Default owner-facing style:

- Chinese, concise, conclusion-first.
- Mention exact file paths and command names in English.
- Avoid hidden chain-of-thought; report decisions, evidence, blockers, and next
  actions.
- For long work, provide short progress updates.
- Final result should include verification evidence and remaining limitations.

Natural language triggers:

- `我的偏好生效了吗`, `aiplus-work-with-you status`, `work-with-you status`,
  `检查我的 AiPlus profile` -> run `aiplus profile status`.
- `secret 状态`, `看看 secret`, `检查 API key`, `API key 是否可用` -> run
  `aiplus secret-broker status` or `aiplus secret-broker doctor`.
- `更新 AiPlus`, `升级 AiPlus`, `update AiPlus` -> run `aiplus update all`.

