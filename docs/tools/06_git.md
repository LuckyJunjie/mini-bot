# Git

## 一、背景

Git 是由 Linus Torvalds 于 2005 年创建的分布式版本控制系统，现已成为软件开发的行业标准。Git 通过 GitHub、GitLab 等平台托管代码，实现协作开发、版本管理和 CI/CD 集成。

## 二、目的

- **版本控制**：追踪代码变更历史
- **协作开发**：多人分支合并管理
- **代码备份**：远程仓库灾备
- **CI/CD 集成**：GitHub Actions 自动构建测试

## 三、官方地址

- 官网: https://git-scm.com/
- 官方文档: https://git-scm.com/doc
- GitHub: https://github.com/
- GitLab: https://gitlab.com/

## 四、实用教程

### 4.1 基础配置

```bash
# 安装 (macOS 自带，Ubuntu 需要安装)
sudo apt install -y git

# 配置用户信息
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 设置默认分支名
git config --global init.defaultBranch main

# 启用颜色
git config --global color.ui auto

# 设置别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate"
```

### 4.2 日常开发流程

```bash
# 克隆仓库
git clone https://github.com/LuckyJunjie/mini-bot.git
cd mini-bot

# 创建功能分支
git checkout -b feature/new-sensor-support

# 开发... 提交
git add .
git commit -m "feat(sensor): add MPU6050 IMU support"

# 推送分支
git push -u origin feature/new-sensor-support

# 在 GitHub 创建 PR，然后合并
```

### 4.3 Mini-Bot 项目协作

```bash
# 1. Fork 仓库（在 GitHub UI 操作）

# 2. 克隆你的 Fork
git clone https://github.com/YOUR_USERNAME/mini-bot.git
cd mini-bot

# 3. 添加上游仓库
git remote add upstream https://github.com/LuckyJunjie/mini-bot.git

# 4. 创建功能分支
git checkout -b docs/bom-update

# 5. 同步上游最新代码
git fetch upstream
git merge upstream/main

# 6. 推送 PR
git push origin docs/bom-update
# 然后在 GitHub 创建 Pull Request
```

### 4.4 GitHub Token 配置

```bash
# 使用 HTTPS + Token 替代密码
git remote set-url origin https://ghp_TOKEN@github.com/LuckyJunjie/mini-bot.git

# 或使用 credential helper
git config --global credential.helper store
# 首次 push 时输入用户名 + Personal Access Token
```

### 4.5 常用命令速查

```bash
# 状态与历史
git status
git log --oneline -10
git diff

# 暂存与提交
git add -A
git commit -m "type: description"
git commit -am "快速提交（仅追踪已管理文件）"

# 分支操作
git branch
git checkout main
git merge feature/xxx
git branch -d feature/xxx

# 远程操作
git fetch origin
git pull origin main
git push origin main
git push -u origin feature/xxx

# 撤销操作
git checkout -- file.txt  # 放弃工作区更改
git reset HEAD file.txt   # 取消暂存
git revert <commit>       # 创建新提交撤销指定提交
```

## 五、替代品

| 替代品 | 特点 |
|--------|------|
| Mercurial (hg) | 简单，BBCode 使用 |
| Subversion (SVN) | 集中式，仍在企业使用 |
| Perforce | 游戏行业，大文件 |

---

*此文档最后更新于 2026-04-09*
