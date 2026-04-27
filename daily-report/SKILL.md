---
name: daily-report
description: Generate a daily work report from git commit history. Use when the user asks for a daily report, work summary, or mentions /daily-report. Supports specifying a date (e.g. "昨天的日报", "4月10日的").
user_invocable: true
---

根据 git 提交记录，生成工作日报。

## 触发方式

用户可以通过以下方式触发：
- `/daily-report` — 生成今天的日报
- `/daily-report 昨天的` — 生成昨天的日报
- `/daily-report 2026-04-10` — 生成指定日期的日报
- 自然语言："帮我写今天的日报"、"生成昨天的工作报告"

## 参数解析

从用户输入中提取目标日期：
- 无参数 → 今天
- "昨天" / "昨天的" / "yesterday" → 昨天
- "YYYY-MM-DD" 格式 → 该日期
- "前天" → 前天

将目标日期转换为 `YYYY-MM-DD` 格式的 `TARGET_DATE`，并计算 `NEXT_DATE`（TARGET_DATE + 1天）。

## 流程

1. 查找所有 git 仓库：搜索 `~/project/` 和 `~/Documents/my-notes/` 下的 `.git` 目录
2. 对每个仓库执行：
   ```
   git log --format="%h %ai %s" --since="TARGET_DATE 00:00:00" --until="NEXT_DATE 00:00:00" --author="$(git config user.name)"
   ```
3. 如提交较少或没有提交，如实记录（如"今日无代码提交，主要进行xxx"），不扩大时间范围
4. 对有提交的仓库执行 `git diff --stat <first_commit>^..<last_commit>` 了解变更范围
5. 结合 CLAUDE.md 中的 workspace 索引理解任务上下文
6. 检查 `/Users/zhaoyue/Documents/my-notes/target/工作日志/YYYYMMDD-report.md` 是否已存在
   - 若不存在：直接创建
   - 若已存在：读取现有内容，与本轮生成的内容对比，去重后追加新内容（保留原有结构，合并"今日完成"等章节）

## 格式

```markdown
# 工作日报 — YYYY-MM-DD

## 项目
<项目名称>

## 今日完成
<按工作类别分组，列出具体完成项>

## 未完成 / 待跟进
<如有>

## 明日计划
<基于 workspace 状态推断>

## 提交记录
共 N 次提交（last_hash ← first_hash）
```

## 注意

- 内容基于 git 事实，不臆造
- workspace 状态从 CLAUDE.md 读取
- 多次执行同一天的报告时，智能合并：已有内容不重复追加，新增内容按章节合并
- 输出文件路径：`/Users/zhaoyue/Documents/my-notes/target/工作日志/daily/YYYYMMDD-report.md`
