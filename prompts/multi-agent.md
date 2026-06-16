---
description: 多 Agent 并行开发 —— 用 git worktree 隔离，互不干扰
argument-hint: "<任务描述>"
---
## 多 Agent 并行开发指令

这是一个多 Agent 协作开发环境。**可能同时有其他 Agent 在同个仓库工作**，必须用 git worktree 隔离，不允许直接修改原项目目录。

### 第一步：确定分支名

从任务描述中自动生成一个简短的分支名（例如 `feat-login`、`fix-header`、`refactor-auth`），存储在变量 `$BRANCH` 中。

### 第二步：创建分支 & worktree（仅首次执行）

```bash
git checkout -b $BRANCH 2>$null || git checkout $BRANCH
```

```bash
$worktree = "../work-$BRANCH"
if (-not (Test-Path $worktree)) { git worktree add $worktree $BRANCH }
```

### 第三步：所有操作在 worktree 内进行

- 工作目录是 `../work-$BRANCH`
- 所有 `bash` 命令用 `cd ../work-$BRANCH && <命令>` 执行
- 所有 `read` / `write` / `edit` 的文件路径以 `../work-$BRANCH/` 开头
- **不要修改原项目目录（CWD）下的任何文件**
- **不要碰其他 Agent 的 worktree**

### 第四步：完成后提交

```bash
cd ../work-$BRANCH
git add -A
git commit -m "feat($BRANCH): $@"
```

### 你的任务

$@
