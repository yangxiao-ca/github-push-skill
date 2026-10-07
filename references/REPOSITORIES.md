# GitHub 仓库列表

本文件列出所有可推送的 GitHub 仓库。Agent 在批量推送时会读取此文件。

## 仓库格式

```markdown
### 仓库名称
- **本地路径**: `/absolute/path/to/repo`
- **远程地址**: `git@github.com:username/repo.git`
- **默认分支**: `main` 或 `master`
- **说明**: 简短描述
```

## 仓库列表

### obsidian-svg-diagrams-skill
- **本地路径**: `/Users/sam/.workbuddy/skills/obsidian-svg-diagrams`
- **远程地址**: `git@github.com:yangxiao-ca/obsidian-svg-diagrams-skill.git`
- **默认分支**: `main`
- **说明**: Obsidian SVG 图表生成 skill

### steelman-thinking
- **本地路径**: `/Users/sam/.workbuddy/skills/steelman-thinking`
- **远程地址**: `git@github.com:yangxiao-ca/steelman-thinking.git`
- **默认分支**: `main`
- **说明**: 双向钢人论证框架 skill（决策前分析、争议拆解）

---

## 添加新仓库

按照上述格式添加新仓库到列表中。Agent 会自动识别并处理。

## 使用说明

- **单仓库推送**: 进入仓库目录后执行 `git push`
- **批量推送**: Agent 会读取此文件，逐个推送所有仓库
- **更新列表**: 随时可以添加新仓库，无需修改 skill 本身
