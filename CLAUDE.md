# CLAUDE.md

此文件为 Claude Code (claude.ai/code) 提供代码库指导。

## 概述

这是个人级别的 Claude Code Skills 仓库，用于在不同设备间同步和共享好用的技能功能。每个子目录是一个独立的 Skill，通过 `/<skill-name>` 调用。

## 目录结构

```
my-skills/
├── context7-mcp/      # Context7 文档查询 Skill
├── daily-report/      # 日报生成 Skill
├── expand-to-article/ # 笔记扩写为技术文章 Skill
├── git-flow/          # Git 提交工作流 Skill
├── proofshot/         # UI 视觉验证 Skill
└── skill-creator/     # Skill 创建和优化工具
```

## Skill 开发

### Skill 目录结构

```
skill-name/
├── SKILL.md (必填)    # YAML frontmatter + Markdown 指令
├── scripts/           # 可执行脚本
├── references/        # 参考文档
├── assets/           # 静态资源
└── agents/           # 子代理指令
```

### 创建新 Skill

1. 在 `skill-creator/` 目录下有完整的创建流程指南
2. 参考现有 Skill 的 SKILL.md 格式
3. 使用 `skill-creator/scripts/package_skill.py` 打包

### Skill 触发机制

Skill 通过 YAML frontmatter 中的 `name` 和 `description` 触发。`description` 是主要触发依据，应该包含：
- Skill 做什么
- 具体的使用场景（包含用户可能的表达方式）

## 常用操作

- `/git-flow` — Git 提交工作流
- `/daily-report` — 生成工作日报
- `/expand-to-article` — 将笔记扩写为技术文章
