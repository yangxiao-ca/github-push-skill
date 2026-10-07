---
name: github-push-skill
description: Push code to GitHub repositories using SSH. Handles git operations including commit, push, and repository management. Use when the user asks to push code, commit changes, or sync with GitHub.
  触发词：推送到GitHub、推代码、push到GitHub、提交到GitHub、同步GitHub、上传代码、推到GitHub、git push、提交代码、推送代码
---

# GitHub Push Skill

使用 SSH 方式推送代码到 GitHub 仓库。支持单仓库推送、多仓库批量推送、自动提交等功能。

## 安全原则

**永远不要在 skill 文件中硬编码 token 或密码。** 本 skill 使用 SSH 密钥进行认证，密钥存储在 `~/.ssh/` 目录。

## 仓库列表

所有可推送的仓库定义在 [references/REPOSITORIES.md](references/REPOSITORIES.md) 中。

## 工作流程

### 1. 单仓库推送

```bash
# 进入仓库目录
cd /path/to/repo

# 检查状态
git status

# 如果有未提交的更改
git add -A
git commit -m "描述性提交信息"

# 推送到远程
git push origin main
```

### 2. 批量推送所有仓库

读取 `references/REPOSITORIES.md` 中的仓库列表，逐个推送：

```bash
# 伪代码
for repo in repositories:
    cd repo.local_path
    git pull --rebase origin main  # 先拉取远程更新
    git push origin main
```

### 3. 自动提交并推送

当用户说"帮我提交并推送"时：

1. 检查当前目录是否是 git 仓库
2. 运行 `git status` 查看更改
3. 询问用户是否要提交所有更改（或让用户指定文件）
4. 生成描述性提交信息
5. 执行 `git add -A && git commit -m "..." && git push`

## SSH 认证配置

### 检查 SSH 密钥

```bash
ls -la ~/.ssh/
# 应该看到 id_ed25519 或 id_rsa 等文件
```

### 测试 GitHub 连接

```bash
ssh -T git@github.com
# 成功会显示：Hi username! You've successfully authenticated...
```

### 添加 SSH 密钥到 GitHub

如果还没有配置：

1. 复制公钥：`cat ~/.ssh/id_ed25519.pub`
2. 访问 https://github.com/settings/keys
3. 点击 "New SSH key"
4. 粘贴公钥并保存

## 常用命令

### 查看远程仓库

```bash
git remote -v
```

### 修改远程仓库地址（切换到 SSH）

```bash
git remote set-url origin git@github.com:username/repo.git
```

### 查看提交历史

```bash
git log --oneline -10
```

### 撤销最后一次提交（保留更改）

```bash
git reset --soft HEAD~1
```

### 强制推送（危险！）

```bash
git push --force origin main
# 只在确定要覆盖远程历史时使用
```

## 错误处理

### 错误：Permission denied (publickey)

**原因**：SSH 密钥未配置或未添加到 GitHub

**解决**：
1. 检查密钥：`ls ~/.ssh/`
2. 测试连接：`ssh -T git@github.com`
3. 添加密钥到 GitHub：https://github.com/settings/keys

### 错误：remote origin already exists

**原因**：远程仓库已配置

**解决**：
```bash
git remote set-url origin git@github.com:username/repo.git
```

### 错误：Updates were rejected because the remote contains work

**原因**：远程有新的提交

**解决**：
```bash
git pull --rebase origin main
git push origin main
```

## 最佳实践

1. **推送前先拉取**：`git pull --rebase origin main`
2. **写描述性提交信息**：不要写 "update" 或 "fix"，要写具体改了什么
3. **小步提交**：不要积累大量更改一次性提交
4. **检查 .gitignore**：确保不推送敏感文件或临时文件
5. **使用分支**：重要功能先在分支开发，合并后再推送

## 参考

- [REPOSITORIES.md](references/REPOSITORIES.md) - 仓库列表
- [GitHub SSH 文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
