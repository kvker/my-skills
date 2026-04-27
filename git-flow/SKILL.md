---
name: git-flow
description: Git commit workflow using conventional commits. Use when the user wants to commit files, asks for "git commit", "提交代码", "git add", or wants to follow a structured commit workflow. Triggers for any git staging/commit/push request.
user_invocable: true
---

使用子代理执行完整 Git 提交流程，避免污染主窗口上下文。

## 参数（push 意愿）

调用 slash 命令时可在 skill 名后指定参数：
- `/git-flow`（无参数）→ 提交后询问是否推送
- `/git-flow y` 或 `/git-flow yes` → 提交后自动推送
- `/git-flow n` 或 `/git-flow no` 或 `/git-flow f` 或 `/git-flow false` → 提交后不推送，直接结束

## 流程

根据用户传入的参数决定推送策略，然后启动 **general-purpose 子代理** 执行完整流程。

### 如果参数为 y/yes
启动子代理，prompt 说明 commit 后**自动推送**：
```
你是 Git 提交流程助手。执行以下完整流程。

### Step 1：确定目标仓库
确认当前工作目录所在仓库。

### Step 2：分析当前变更
执行 `git status` 获取所有变更文件（unstaged + staged + untracked）。
展示变更文件列表。

### Step 3：自动暂存文件
根据上下文自动推断要提交的文件并暂存，不询问用户。

### Step 4：自动确定 commit 类型和描述
自动推断，不询问用户。

| 变更内容 | type |
|----------|------|
| 新增功能代码、模块 | feat |
| 仅文档、注释修改 | doc |
| 性能优化、代码优化 | opt |
| 重构、整理代码 | refactor |
| Bug 修复 | fix |

描述由你根据变更内容生成。格式：`type: description`

### Step 5：执行 commit
执行 `git commit -m "type: description"`。

### Step 6：自动推送
commit 成功后**直接执行 `git push`**，不询问用户。
```

### 如果参数为 n/no/f/false（明确拒绝推送）
启动子代理，prompt 说明 commit 后**直接结束**：
```
你是 Git 提交流程助手。执行以下完整流程。

### Step 1：确定目标仓库
确认当前工作目录所在仓库。

### Step 2：分析当前变更
执行 `git status` 获取所有变更文件（unstaged + staged + untracked）。
展示变更文件列表。

### Step 3：自动暂存文件
根据上下文自动推断要提交的文件并暂存，不询问用户。

### Step 4：自动确定 commit 类型和描述
自动推断，不询问用户。

| 变更内容 | type |
|----------|------|
| 新增功能代码、模块 | feat |
| 仅文档、注释修改 | doc |
| 性能优化、代码优化 | opt |
| 重构、整理代码 | refactor |
| Bug 修复 | fix |

描述由你根据变更内容生成。格式：`type: description`

### Step 5：执行 commit
执行 `git commit -m "type: description"`。

### Step 6：结束
commit 成功后**直接结束**，不推送，不询问用户。
```

### 如果无参数（默认行为）
启动子代理，prompt 说明 commit 后**询问用户**：
```
你是 Git 提交流程助手。执行以下完整流程。

### Step 1：确定目标仓库
确认当前工作目录所在仓库。

### Step 2：分析当前变更
执行 `git status` 获取所有变更文件（unstaged + staged + untracked）。
展示变更文件列表。

### Step 3：自动暂存文件
根据上下文自动推断要提交的文件并暂存，不询问用户。

### Step 4：自动确定 commit 类型和描述
自动推断，不询问用户。

| 变更内容 | type |
|----------|------|
| 新增功能代码、模块 | feat |
| 仅文档、注释修改 | doc |
| 性能优化、代码优化 | opt |
| 重构、整理代码 | refactor |
| Bug 修复 | fix |

描述由你根据变更内容生成。格式：`type: description`

### Step 5：执行 commit
执行 `git commit -m "type: description"`。

### Step 6：询问是否推送
commit 成功后询问：`是否推送到远程？(yes/no)`
- yes → 执行 `git push`
- no → 结束
```

## 注意
- commit message 只需一行
- 如果没有变更文件，提示用户并结束
