# GitHub Push Skill

使用 SSH 方式推送代码到 GitHub 仓库的 Agent Skill。支持单仓库推送、多仓库批量推送、自动提交等功能。

## 功能特性

- **SSH 认证**：使用 SSH 密钥，不存储 token 或密码
- **单仓库推送**：进入仓库目录后执行推送
- **批量推送**：读取仓库列表，逐个推送所有仓库
- **自动提交**：检查更改、生成提交信息、推送到远程
- **错误处理**：处理常见 Git 错误并提供解决方案

## 安装方式

### 方式 1：软链接到各个 Agent（推荐）

```bash
# 源目录
SOURCE="/Users/sam/.workbuddy/skills/github-push-skill"

# 创建软链接到各个 Agent
ln -s "$SOURCE" ~/.workbuddy/skills/github-push-skill
ln -s "$SOURCE" ~/.claude/skills/github-push-skill
ln -s "$SOURCE" ~/.codex/skills/github-push-skill
ln -s "$SOURCE" ~/.opencode/skills/github-push-skill
```

### 方式 2：直接克隆（如果需要独立副本）

```bash
# 克隆到源目录
git clone git@github.com:yangxiao-ca/github-push-skill.git \
  ~/.agents/skills/github-push-skill

# 然后做软链接
ln -s ~/.agents/skills/github-push-skill ~/.workbuddy/skills/github-push-skill
ln -s ~/.agents/skills/github-push-skill ~/.claude/skills/github-push-skill
ln -s ~/.agents/skills/github-push-skill ~/.codex/skills/github-push-skill
ln -s ~/.agents/skills/github-push-skill ~/.opencode/skills/github-push-skill
```

## 触发词

当你在对话中提到以下关键词时，Agent 会自动加载这个 skill：

- 推送到GitHub
- 推代码
- push到GitHub
- 提交到GitHub
- 同步GitHub
- 上传代码
- 推到GitHub
- git push
- 提交代码
- 推送代码

## 使用示例

```bash
# 单仓库推送
用户：帮我把这个代码推送到 GitHub
Agent：检查 git 状态 → 提交更改 → 推送到远程

# 批量推送
用户：把所有仓库都推送到 GitHub
Agent：读取 REPOSITORIES.md → 逐个推送

# 自动提交
用户：帮我提交并推送
Agent：git status → git add -A → git commit → git push
```

## 文件结构

```
github-push-skill/
├── SKILL.md                    # 主技能文件
├── README.md                   # 本文件
└── references/
    └── REPOSITORIES.md         # 仓库列表
```

## 安全原则

**永远不要在 skill 文件中硬编码 token 或密码。**

本 skill 使用 SSH 密钥进行认证：
- 密钥存储在 `~/.ssh/` 目录
- 公钥添加到 GitHub：https://github.com/settings/keys
- 测试连接：`ssh -T git@github.com`

## 仓库列表

所有可推送的仓库定义在 [references/REPOSITORIES.md](references/REPOSITORIES.md) 中。

当前仓库：
- **obsidian-svg-diagrams-skill**: `/Users/sam/.workbuddy/skills/obsidian-svg-diagrams`

添加新仓库只需编辑 `REPOSITORIES.md`，无需修改 skill 本身。

## 常用命令

```bash
# 查看远程仓库
git remote -v

# 切换到 SSH 地址
git remote set-url origin git@github.com:username/repo.git

# 推送前先拉取
git pull --rebase origin main

# 强制推送（危险！）
git push --force origin main
```

## 错误处理

### Permission denied (publickey)
- 检查 SSH 密钥：`ls ~/.ssh/`
- 测试连接：`ssh -T git@github.com`
- 添加公钥到 GitHub

### Updates were rejected
- 先拉取远程更新：`git pull --rebase origin main`
- 再推送：`git push origin main`

### remote origin already exists
- 修改远程地址：`git remote set-url origin git@github.com:username/repo.git`

## 最佳实践

1. **推送前先拉取**：避免冲突
2. **写描述性提交信息**：不要写 "update" 或 "fix"
3. **小步提交**：不要积累大量更改
4. **检查 .gitignore**：确保不推送敏感文件
5. **使用分支**：重要功能先在分支开发

## 仓库地址

https://github.com/yangxiao-ca/github-push-skill

## 许可证

MIT
