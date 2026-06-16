---
description: 多 Agent 并行开发 —— 用 git worktree 隔离，互不干扰
argument-hint: "<分支名> <任务描述>"
---
## 多 Agent 并行开发指令

这是一个多 Agent 协作开发环境。**可能同时有其他 Agent 在同个仓库工作**，必须用 git worktree 隔离，不允许直接修改原项目目录。

### 第一步：创建分支 & worktree（仅首次执行）

```bash
# 创建分支（已存在则跳过）
git checkout -b $1 2>$null || git checkout $1

# 创建 worktree 到上级目录
$worktree = "../work-$1"
if (-not (Test-Path $worktree)) {
  git worktree add $worktree $1
}
```

### 第二步：所有操作在 worktree 内进行

- 项目根目录是 CWD 的 `../work-$1` 目录
- 所有 `bash` 命令请用 `cd ../work-$1 && <命令>` 的形式执行
- 所有 `read` / `write` / `edit` 操作，文件路径必须以 `../work-$1/` 开头
- **不要修改原项目目录（CWD）下的任何文件**
- **不要碰其他 Agent 的 worktree 目录**

### 第三步：完成后提交

```bash
cd ../work-$1
git add -A
git commit -m "feat($1): $2"
```

### 你的任务

$2
