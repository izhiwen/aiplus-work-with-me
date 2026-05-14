# AiPlus work-with-you

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[English README](README.md)

为 AiPlus 设计的便携式用户 profile bundle。解决跨项目、跨 session 的失忆问题：
让 agent 记住 *你* 的协作风格、项目地图、角色身份、工具偏好，而不需要你每次新
session、每次新仓库都重新讲一遍。

## 痛点

周一你在 A 项目对 agent 说：

> "用中文回；先结论后理由；不要不打招呼就 push；我们用 LIGHT/MEDIUM/HEAVY 分层。"

周二你打开 B 项目，开一个新 session。Agent 完全不记得你。它写了一堆英文摘要，
建议直接 push 到 main，还问什么是 "MEDIUM tier"。

正确的解法不是 "把所有偏好再讲一遍"。正确的解法是：一份独立于具体项目的、结构化
的、便携的 profile —— 它由你掌控；任何 AiPlus-aware 的 agent 在你 cd 进项目时
都能读到。

## 这是什么

`AiPlus-Work-with-Me` 就是这份 profile bundle 的**模板仓库**。你 fork 一份、
填好占位符、用 `aiplus profile install` 安装一次，从此机器上所有 AiPlus-aware
agent、所有项目都共享同一套 "如何与你协作" 的理解。

它保存五类内容：

- **`USER.md`** —— 稳定的 Owner 信息：姓名、主工作目录、语言、决策权限、危险动作 gates。
- **`preferences/`** —— 五个主题的偏好文档：communication、engineering、CEO/review、
  tools-and-runtimes、privacy-and-secrets。
- **`identities/`** —— 七个角色身份契约（CEO / Advisor / Rust Lead / Runtime QA /
  Security Reviewer / Reviewer / Builder）。可复用的角色脚手架。
- **`sync/`** —— 跨项目同步策略、项目注册表、提升规则、冲突/陈旧处理。决定哪些是
  通用偏好、哪些只属于单个项目。
- **`MEMORY.md`** —— memory taxonomy 和 remember / sync / forget / supersede /
  reject 规则，定义偏好如何随时间沉淀。

它**不保存**任何 secret：没有 API key、没有 Bitwarden token、没有 provider
payload、没有 transcript、没有项目文件、没有 compact checkpoint。只保存别名和
策略。详见 [SECURITY.md](SECURITY.md)。

## 安装

```bash
# 1. Fork 或 clone 仓库到本地（不要放在某个具体项目目录下）
git clone https://github.com/<your-account>/AiPlus-Work-with-Me.git
cd AiPlus-Work-with-Me

# 2. 填好占位符
#    USER.md            —— 姓名、工作目录、语言、gates
#    profile.toml       —— owner 字段、defaults
#    sync/projects.toml —— 你本地的项目路径和 scope
#    docs/project-map.md
#    secret-aliases.tsv —— 你的 Bitwarden 别名命名空间

# 3. 安装为 user-level AiPlus profile
aiplus profile install AiPlus-Work-with-Me --user --yes
```

验证：

```bash
aiplus profile status
aiplus profile doctor
aiplus refresh
aiplus status
aiplus doctor
```

## Agent Continuity 自然语言入口

当 AiPlus-aware agent 加载本 profile 后，以下中英文表达会映射到对应 memory 操作：

| 你说 | Agent 做 |
| --- | --- |
| 记住这个 / "remember this" | 脱敏后写 project-local memory |
| 记住这个偏好 / "remember this preference" | 创建 profile 层 GLOBAL_PREFERENCE candidate |
| 以后都这样 / "from now on" | 创建 profile/global candidate，不自动批准 |
| 只在这个项目用 / "only in this project" | 仅写 project memory |
| 忘掉这个 / "forget this" | 按 memory id forget；不明确时先问 |
| 你记住了什么 / "what do you remember" | memory status |
| 这次用了哪些记忆 / "which memories were used" | memory context |
| 新开顾问 / "new advisor" | 加载 advisor identity context |
| 新开 CEO / "new CEO" | 加载 CEO identity context |
| 把这次经验沉淀成 skill / "make this a skill" | 创建 Skill Candidate（不是 approved skill）|
| 不要用我的私人记忆 / 本次忽略我的偏好 | 本 session opt-out |

## 边界 —— 它**不是**什么

本 profile **描述**偏好，不**授予**权限。

Memory 是 context，不是 instruction。Identity 是 role contract，不是 permission。
Skill Candidate 是 proposal，不是 approved skill。本 profile **不能**授权：

- push、publish、release、deploy
- secret 访问或 secret exposure
- 全局 agent 配置或 shell 配置变更
- 外部账号变更

任何危险动作仍然需要 live session 里明确的 Owner 消息。详见
[docs/owner-gates.md](docs/owner-gates.md)。

## 与其他 AiPlus 模块的关系

`AiPlus-Work-with-Me` 是 user-profile bundle 层。它与以下模块组合使用：

- **`aiplus-agent-memory`** —— project-local JSONL memory 引擎。本 profile 设定
  taxonomy 和 sync 规则；agent-memory 负责把实际 records 存到每个项目的
  `.aiplus/memory/`。
- **`aiplus-secret-broker`** —— 运行时通过密码管理器拿 secret。本 profile 只列
  alias 名称；broker 在运行时从 Bitwarden 等地方拿真实值。
- **`aiplus-auto-compact`** —— compact / handoff 工作流。
- **`aiplus-auto-team-consultant`** —— 多 agent 咨询工作流。

## Fork 工作流

本仓库设计上就是 **fork → 填好个人内容 → 转私有** —— 你填好的 `USER.md`、
`MEMORY.md`、`preferences/`、`sync/projects.toml` 可能含有你不想公开的个人 context。

推荐流程：

```bash
# 在 GitHub 上 fork 本公开模板 → clone 你的私有 fork
# （或重命名为 aiplus-work-with-<你的名字>，并把可见性切到 private）
gh repo fork izhiwen/AiPlus-Work-with-Me --clone --fork-name aiplus-work-with-<你的名字>
gh repo edit izhiwen/aiplus-work-with-<你的名字> --visibility private
```

不想留 fork 关系的话，直接下载 tarball 也可以。

## License

Apache-2.0。详见 [LICENSE](LICENSE)。
