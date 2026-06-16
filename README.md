# pi-workflows

Pi 多 Agent 并行开发工作流模板包。

## 安装

```bash
pi install git:github.com/MuziIsabel/pi-workflows
```

## 模板

### `/multi-agent` — 多 Agent 并行开发

在同一个 git 仓库中用多个 pi agent 并行工作，互不干扰。

原理：每个 agent 创建独立的 git 分支 + worktree，物理隔离文件系统。

**用法：**

```bash
# 终端 A — 不指定分支名，自动生成
cd your-project
pi
> /multi-agent 实现邮箱密码登录

# 终端 B
cd your-project
pi
> /multi-agent 实现仪表盘页面
```

模板会自动：
1. 从任务描述中生成分支名（如 `feat-login`）
2. 创建分支 & worktree 到 `../work-<分支名>`
3. 所有文件操作限制在 worktree 内
4. 完成后自动提交

最后在主分支合并：

```bash
git merge feat-login
git merge feat-dash
```
