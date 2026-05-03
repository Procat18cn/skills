# Skills

个人 Agent Skills 集合，遵循 [Agent Skills 开放标准](https://agentskills.io)。

## 目录

- [docs/skills-getting-started.md](docs/skills-getting-started.md) — Agent Skills 入门指南
- [skills/](skills/) — Skill 存放目录

## 使用方式

将 Skill 符号链接到目标工具的发现路径即可：

```bash
# 以 Claude Code 为例
ln -s $(pwd)/skills/organize-tech-knowledge-doc ~/.claude/skills/organize-tech-knowledge-doc
```

详见 [入门指南](docs/skills-getting-started.md)。
