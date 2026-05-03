# AGENTS.md

Skills 仓库，遵循 [Agent Skills 开放标准](https://agentskills.io)。所有 Skill 存放在 `skills/` 目录下。

## 仓库约定

- 每个 Skill 是 `skills/<skill-name>/` 下的独立文件夹，文件夹名与 SKILL.md 中的 `name` 字段一致
- Skill 目录结构：`SKILL.md`（必需）+ `references/`、`scripts/`、`assets/`（可选）
- 文档（教程、指南）放在 `docs/`，不放在 `skills/`

## Skill 编写规范

**Description 字段**：精确列出触发场景和关键词。宁可稍显冗余，不可过于笼统。Agent 只靠 description 判断是否触发 Skill。

**正文**：保持 500 行以内，工具无关。详细模板、反模式、检查清单放入 `references/` 按需加载。

**措辞**：祈使句，不引用特定工具 API。写"读取源文件"而非"使用 Read 工具"；写"搜索网页"而非"调用 WebSearch"。

**YAML frontmatter**：
```yaml
---
name: skill-name          # 小写+数字+连字符，与目录名一致
description: >-           # 1-1024 字符，明确说明做什么、何时触发
  ...
license: MIT
---
```

## 新增 Skill 步骤

1. 在 `skills/` 下创建 `<skill-name>/` 文件夹
2. 编写 `SKILL.md`（参考 `docs/skills-getting-started.md`）
3. 如有补充材料，放入 `references/`
4. 提交前自检：
   - [ ] `name` 字段与目录名一致，符合 `^[a-z0-9]+(-[a-z0-9]+)*$`
   - [ ] `description` 非空，明确列出触发场景和关键词
   - [ ] 正文无硬编码的工具名引用（Read/Bash/WebFetch 等）
   - [ ] 正文控制在 500 行以内，详细内容已推入 `references/`
   - [ ] 措辞使用祈使句，无建议性语气

## 安装与使用

将 Skill symlink 到目标工具的发现路径：

```bash
# Claude Code
ln -s $(pwd)/skills/<skill-name> ~/.claude/skills/<skill-name>
# Codex
ln -s $(pwd)/skills/<skill-name> ~/.agents/skills/<skill-name>
```

详见 `docs/skills-getting-started.md`。
