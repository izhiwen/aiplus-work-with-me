# Communication Preferences

Owner-facing communication style and natural-language trigger mappings.

## Default Style

- Language: Chinese for natural language; English for technical terms.
- Tone: Concise, conclusion-first, evidence-based.
- Avoid hidden chain-of-thought in final responses.
- Report decisions, evidence, blockers, and next actions only.
- Avoid emojis unless explicitly requested.

## Response Structure

- Short progress updates for long work.
- Final response must include: verdict (PASS/REVISE/BLOCKED), evidence, limitations, next actions.
- Use exact English for: file paths, commands, schema keys, status enums, repo names, model names, secret aliases.

## Natural Language Triggers

| Owner Says | Action |
|------------|--------|
| "我的偏好生效了吗", "AiPlus-Work-with-Me status", "work-with-you status", "检查我的 AiPlus profile" | Run `aiplus profile status` |
| "secret 状态", "看看 secret", "检查 API key", "API key 是否可用" | Run `aiplus secret-broker status` or `aiplus secret-broker doctor` |
| "更新 AiPlus", "升级 AiPlus", "update AiPlus" | Run `aiplus update all` |
| "保存进度", "准备 compact", "做个交接" | Run `aiplus compact prepare` |
| "继续", "resume", "refresh", "刷新" after compact | Run `aiplus compact resume` when appropriate |
| "记住这个" / "记住这个偏好" | Add redacted project memory |
| "以后都这样" | Create profile/global candidate only; do not silently approve |
| "只在这个项目用" | Project memory only |
| "忘掉这个" | Forget by memory ID; ask which ID if ambiguous |
| "你记住了什么" / "这次用了哪些记忆" | Memory status and context |
| "把这次经验沉淀成 skill" | Create Skill Candidate only |
| "不要用我的私人记忆" / "本次忽略我的偏好" | Session-local opt-out |

## Language Policy

- English is authoritative for execution rules, tool policy, schemas, enums, STOP conditions, agent handoffs, and output contracts.
- Chinese is authoritative for Owner-facing natural-language responses.
- If English and Chinese conflict, follow the English execution rule, except for Owner-facing language and format requirements.

## Session Opt-Out

- "本次忽略我的偏好", "关闭 work-with-you", "只看项目规则" -> ignore this profile for current session.
- Opt-out is session-local only. Next session reverts to normal profile loading.
