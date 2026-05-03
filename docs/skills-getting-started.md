# Agent Skills 入门指南

## 什么是 Agent Skill？

Agent Skill 是一个**可复用的、自包含的能力包**，用于指导 AI 编码 Agent（如 Claude Code、Codex、Cursor、Copilot 等）完成特定类型的任务。

一个 Skill 本质上是一个文件夹，包含一条核心指令文件（`SKILL.md`）和可选的脚本、参考文档、模板等附属资源。Agent 在启动时自动发现已安装的 Skill，并在匹配到相关任务时按需加载。

### 为什么需要 Skill？

| 问题 | Skill 如何解决 |
|------|---------------|
| 每次让 Agent 做类似任务都要重复描述要求 | 将工作流固化为 Skill，一次编写、反复使用 |
| Agent 不了解特定领域的规范或约定 | 在 Skill 中编码领域知识和质量检查清单 |
| 团队协作时各人写法不一致 | Skill 确保同一任务产出风格统一的输出 |
| 跨项目、跨工具的技能无法复用 | 符合开放标准的 Skill 可跨 26+ 工具共享 |

---

## 核心标准：Agent Skills 开放规范

Agent Skills 规范由 Anthropic 发起，现由 Linux Foundation 下的 Agentic AI Foundation 维护。该项开放标准已被 26+ 个 AI 编码工具支持。

### 目录结构

```
my-skill/
├── SKILL.md          # 必需 — YAML frontmatter + Markdown 指令
├── scripts/          # 可选 — 确定性执行的脚本（Python、Bash 等）
├── references/       # 可选 — 按需加载的补充文档
└── assets/           # 可选 — 模板、图片、数据文件等静态资源
```

### SKILL.md 格式

```markdown
---
name: my-skill-name           # 必需：1-64 字符，小写字母+数字+连字符
description: >-               # 必需：最多 1024 字符，描述功能与触发场景
  简要描述这个 Skill 做什么、何时使用。
  当用户要求 X、Y、Z 时触发。
license: MIT                  # 可选：SPDX 标识
compatibility:                # 可选：环境要求（最多 500 字符）
  python: ">=3.10"
---

# Skill 标题

## 工作流
1. 步骤一...
2. 步骤二...

## 规则
- 约束条件一
- 约束条件二
```

### Name 字段约束

- 正则：`^[a-z0-9]+(-[a-z0-9]+)*$`
- 最长 64 字符
- 只允许小写字母、数字、连字符
- **必须与父目录名一致**

---

## 渐进式加载：保护 Context Window

Agent Skills 的核心设计之一是**三级渐进式加载**，确保安装大量 Skill 也不会挤占上下文窗口：

| 层级 | 加载内容 | 加载时机 | Token 成本 |
|------|---------|---------|-----------|
| **1. 元数据** | `name` + `description`（约 50 tokens） | 会话启动时，加载所有已安装 Skill | 极低 |
| **2. 完整指令** | `SKILL.md` 全部正文 | Agent 判断 Skill 与当前任务相关时 | 中（建议 <5000 tokens） |
| **3. 附属资源** | `references/`、`scripts/`、`assets/` | 执行过程中用到时才读取 | 按需 |

这意味着你可以安装**上百个 Skill**，性能几乎不受影响。

### 设计启示

- **Description 是唯一的触发入口** — 写精确、写"pushy"（明确列出触发关键词），模糊的描述是 Skill 不被正确触发的首要原因
- **SKILL.md 保持精简** — 正文控制在 200-500 行以内，详细内容推到 `references/` 中按需引用
- **Reference 文件按主题拆分** — 一个大的 reference.md 不如多个小的（如 `template.md`、`checklist.md`、`anti-patterns.md`）

---

## 跨工具安装

各工具的 Skill 发现路径不同，但都遵循相同的目录结构约定：

| 工具 | 项目级路径 | 用户级路径 |
|------|-----------|-----------|
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| OpenAI Codex | `.agents/skills/` | `~/.agents/skills/` |
| GitHub Copilot | `.github/skills/` | `~/.copilot/skills/` |
| Cursor | `.cursor/skills/` | — |
| OpenCode | `.opencode/skills/` | — |
| Windsurf | `.windsurf/skills/` | — |

### 推荐方式：Symlink 共享

维护一份权威 Skill 源，通过符号链接安装到各工具的发现路径：

```bash
# 假设 Skill 源仓库位于 ~/shared-agent-skills
SKILL_NAME="organize-tech-knowledge-doc"

# 安装到 Claude Code
ln -s ~/shared-agent-skills/skills/$SKILL_NAME ~/.claude/skills/$SKILL_NAME

# 安装到 Codex
ln -s ~/shared-agent-skills/skills/$SKILL_NAME ~/.agents/skills/$SKILL_NAME

# 安装到 OpenCode
ln -s ~/shared-agent-skills/skills/$SKILL_NAME ~/.opencode/skills/$SKILL_NAME
```

也可以写一个 `install.sh` 脚本自动处理：

```bash
#!/bin/bash
SKILLS_DIR="$(cd "$(dirname "$0")/skills" && pwd)"
TOOLS=("$HOME/.claude/skills" "$HOME/.agents/skills" "$HOME/.opencode/skills")

for skill_dir in "$SKILLS_DIR"/*/; do
  skill_name=$(basename "$skill_dir")
  for tool_path in "${TOOLS[@]}"; do
    mkdir -p "$tool_path"
    ln -sf "$skill_dir" "$tool_path/$skill_name"
  done
done
```

---

## 编写规范

### 1. 从评估出发

先让 Agent 在没有 Skill 的情况下完成任务，观察哪里反复出错或不够好，再针对性地编写 Skill。

### 2. Description 要"pushy"

Description 是 Agent 自动判断是否应用 Skill 的主要依据。**宁可稍显冗余，也不要过于笼统**。

```yaml
# ❌ 太模糊
description: Helps with documentation.

# ✅ 精确，列出触发场景
description: >-
  Organize scattered technical materials into structured knowledge documents.
  Use when the user asks to summarize, distill, restructure, or organize
  technical content from files, web pages, PDFs, chat logs, or notes.
```

### 3. 保持工具无关

SKILL.md 正文不应引用特定工具名或 API。说"读取源文件"而非"使用 Read 工具"，说"搜索网页"而非"调用 WebSearch"。

### 4. 使用祈使句

写"检查：`command`"而非"你可能想运行..."。直接、确定性的指令比建议性语气更能让 Agent 产生一致的行为。

### 5. 包含具体示例

在 `references/` 或 `assets/` 中提供期望输出的示例，帮助 Agent 校准质量。

### 6. 隔离破坏性操作

对于部署、提交、删除等不可逆操作，使用 `disable-model-invocation: true`（Claude Code 扩展字段），要求用户手动调用。

### 7. 脚本优先于生成

确定性的操作（排序、解析、格式转换）应编写 `scripts/` 中的脚本来执行，而非依赖 LLM 的 token 生成，后者容易出错且不可靠。

### 8. 迭代优化

每次使用 Skill 后，审视产出质量，将发现的问题和改进捕获到 Skill 中。

---

## 最小示例

一个完整的、可直接使用的最小 Skill：

```
code-review/
└── SKILL.md
```

```markdown
---
name: code-review
description: >-
  Structured code review for correctness, security, performance, and
  readability. Use when asked to review a diff, PR, or changes for merge.
license: MIT
---

# Code Review

## Process
1. Identify the goal of the change
2. Review for correctness (edge cases, error handling)
3. Review for security (authz, injection risks, secrets)
4. Review for performance (N+1 queries, hot paths)
5. Review for maintainability (naming, structure, tests)
6. Summarize findings: must-fix items, suggestions, and positives

## Rules
- Never approve code that introduces plaintext secrets
- Flag missing tests for new behavior as a must-fix
- Prefer suggestions over demands for style-only issues
```

---

## 验证 Skill 是否符合规范

写完 Skill 后，可以用以下方式检查是否符合规范，按需选用：

### 方法一：自检清单（零依赖）

对照以下清单自行检查。其中 ①–③ 来自 Agent Skills 规范要求，④–⑤ 是跨工具可移植的实践建议：

- ① `name` 字段是否与目录名一致？格式是否为小写字母+数字+连字符（正则 `^[a-z0-9]+(-[a-z0-9]+)*$`）？
- ② `description` 是否非空？是否清楚说明了 Skill 做什么、何时触发？
- ③ 正文是否控制在 500 行以内？详细内容是否已放入 `references/`？
- ④ 正文有没有不小心写了特定工具名（如 "Read 工具"、"WebSearch"），破坏了跨工具可移植性？
- ⑤ 措辞是否使用祈使句（"检查：..."），而非建议性语气（"你可能想..."）？

完整约定见本仓库 `AGENTS.md`。

### 方法二：用 Shell 脚本自动检查

对于 `name` 格式、目录一致性、行数等机械检查，可以写一个简单的 shell 脚本自动跑，几行 `grep` + `wc` 就能搞定。不少个人 Skills 仓库（如 Olshansk/agent-skills）用 Makefile 做这件事。

### 方法三：用 `skills-ref` 验证器

Agent Skills 规范附带了一个参考验证器，能检查 frontmatter 字段、token 预算等。安装方式任选其一：

```bash
# 方式 A：uv（推荐，无需配置 Python 环境）
# 先安装 uv：curl -LsSf https://astral.sh/uv/install.sh | sh
uvx --from git+https://github.com/agentskills/agentskills#subdirectory=skills-ref \
  skills-ref validate ./my-skill

# 方式 B：pip + venv（适合已有 Python 环境的用户）
python3 -m venv .venv && source .venv/bin/activate
pip install git+https://github.com/agentskills/agentskills#subdirectory=skills-ref
skills-ref validate ./my-skill

# 常用命令
skills-ref validate <path>         # 检查结构、frontmatter、token 预算
skills-ref to-prompt <path>        # 生成 prompt 格式
```

---

## 本仓库组织方式

```
.
├── AGENTS.md                       # 仓库约定与 Skill 编写规范
├── docs/
│   └── skills-getting-started.md   # 本文档
├── skills/                         # 所有 Skill 的存放目录
│   └── organize-tech-knowledge-doc/
│       ├── SKILL.md                # 核心指令
│       └── references/
│           └── template.md         # 文档结构详细模板
├── LICENSE                         # MIT
└── README.md
```

每个 Skill 是 `skills/` 下的一个独立文件夹，符合 Agent Skills 开放标准。要使用某个 Skill，将其 symlink 到目标工具的 Skill 发现路径即可。

---

## 参考资源

- [Agent Skills 规范](https://agentskills.io) — 开放标准详细说明
- [Anthropic 官方公告](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills) — Agent Skills 的设计理念与实践
- [Anthropic Skills 仓库](https://github.com/anthropics/skills) — 官方 Skill 示例集合
- [Hugging Face Skills](https://github.com/huggingface/skills) — 社区 Skill 大集合
- [Awesome Agent Skills](https://github.com/kodustech/awesome-agent-skills) — 精选 Skill 列表
- [skills-ref 验证器](https://github.com/agentskills/agentskills) — 标准参考验证工具
- [AGENTS.md & SKILL.md 完全指南 (2026)](https://www.morphllm.com/agents-md-guide)
