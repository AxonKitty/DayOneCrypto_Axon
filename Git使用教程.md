# Git 使用教程

## 目录
1. [Git 简介](#git-简介)
2. [安装 Git](#安装-git)
3. [基础配置](#基础配置)
4. [基本概念](#基本概念)
5. [常用命令](#常用命令)
6. [分支管理](#分支管理)
7. [远程仓库](#远程仓库)
8. [实用技巧](#实用技巧)
9. [常见问题](#常见问题)

---

## Git 简介

Git 是一个分布式版本控制系统,用于跟踪文件的变化,协调多人协作开发。

### 为什么使用 Git?
- ✅ 跟踪代码变更历史
- ✅ 多人协作开发
- ✅ 回退到任意历史版本
- ✅ 创建分支进行实验性开发
- ✅ 代码备份和恢复

---

## 安装 Git

### macOS
```bash
# 使用 Homebrew 安装
brew install git

# 或者从官网下载安装包
# https://git-scm.com/download/mac
```

### 验证安装
```bash
git --version
```

---

## 基础配置

### 设置用户信息
```bash
# 设置用户名
git config --global user.name "你的名字"

# 设置邮箱
git config --global user.email "your.email@example.com"

# 查看配置
git config --list
```

### 配置默认编辑器
```bash
# 设置 VS Code 为默认编辑器
git config --global core.editor "code --wait"

# 或设置 vim
git config --global core.editor "vim"
```

### 配置别名(可选)
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

---

## 基本概念

### 三个工作区域
1. **工作目录 (Working Directory)**: 你实际编辑文件的地方
2. **暂存区 (Staging Area)**: 临时存储即将提交的更改
3. **仓库 (Repository)**: Git 存储所有版本历史的地方

### 文件状态
- **未跟踪 (Untracked)**: 新文件,Git 还不知道
- **已修改 (Modified)**: 文件已更改但未暂存
- **已暂存 (Staged)**: 文件已添加到暂存区
- **已提交 (Committed)**: 文件已保存到仓库

---

## 常用命令

### 1. 创建仓库

#### 初始化新仓库
```bash
# 在当前目录创建 Git 仓库
git init

# 创建新目录并初始化
git init my-project
cd my-project
```

#### 克隆现有仓库
```bash
# 克隆远程仓库
git clone https://github.com/username/repo.git

# 克隆到指定目录
git clone https://github.com/username/repo.git my-folder
```

### 2. 查看状态

```bash
# 查看当前状态
git status

# 简洁模式
git status -s
```

### 3. 添加文件到暂存区

```bash
# 添加单个文件
git add filename.txt

# 添加所有修改的文件
git add .

# 添加所有 .js 文件
git add *.js

# 交互式添加
git add -p
```

### 4. 提交更改

```bash
# 提交暂存区的文件
git commit -m "提交说明"

# 添加并提交(跳过 git add)
git commit -am "提交说明"

# 修改上一次提交
git commit --amend
```

### 5. 查看历史

```bash
# 查看提交历史
git log

# 简洁单行显示
git log --oneline

# 图形化显示分支
git log --oneline --graph --all

# 查看最近 5 次提交
git log -5

# 查看某个文件的历史
git log filename.txt

# 查看详细变更
git log -p
```

### 6. 查看差异

```bash
# 查看工作目录与暂存区的差异
git diff

# 查看暂存区与最后一次提交的差异
git diff --staged

# 查看两个提交之间的差异
git diff commit1 commit2
```

### 7. 撤销操作

```bash
# 撤销工作目录的修改(危险!)
git checkout -- filename.txt

# 从暂存区移除文件(保留工作目录的修改)
git reset HEAD filename.txt

# 撤销最后一次提交(保留更改)
git reset --soft HEAD^

# 撤销最后一次提交(不保留更改,危险!)
git reset --hard HEAD^

# 恢复到特定提交
git reset --hard commit_hash
```

### 8. 删除和移动文件

```bash
# 删除文件
git rm filename.txt

# 仅从 Git 删除,保留本地文件
git rm --cached filename.txt

# 移动/重命名文件
git mv old_name.txt new_name.txt
```

---

## 分支管理

### 分支基础

```bash
# 查看所有分支
git branch

# 查看远程分支
git branch -r

# 查看所有分支(包括远程)
git branch -a

# 创建新分支
git branch feature-login

# 切换分支
git checkout feature-login

# 创建并切换分支(推荐)
git checkout -b feature-login

# 使用新语法(Git 2.23+)
git switch feature-login
git switch -c feature-login
```

### 合并分支

```bash
# 切换到目标分支
git checkout main

# 合并指定分支到当前分支
git merge feature-login

# 取消合并
git merge --abort
```

### 删除分支

```bash
# 删除已合并的分支
git branch -d feature-login

# 强制删除分支
git branch -D feature-login

# 删除远程分支
git push origin --delete feature-login
```

### 变基 (Rebase)

```bash
# 将当前分支变基到 main
git rebase main

# 交互式变基(整理提交历史)
git rebase -i HEAD~3

# 继续变基
git rebase --continue

# 取消变基
git rebase --abort
```

---

## 远程仓库

### 查看远程仓库

```bash
# 查看远程仓库
git remote

# 查看详细信息
git remote -v

# 查看远程仓库详情
git remote show origin
```

### 添加远程仓库

```bash
# 添加远程仓库
git remote add origin https://github.com/username/repo.git

# 修改远程仓库 URL
git remote set-url origin https://github.com/username/new-repo.git

# 删除远程仓库
git remote remove origin
```

### 推送到远程

```bash
# 推送到远程仓库
git push origin main

# 首次推送并设置上游分支
git push -u origin main

# 推送所有分支
git push --all origin

# 推送标签
git push --tags
```

### 从远程拉取

```bash
# 获取远程更新(不合并)
git fetch origin

# 拉取并合并
git pull origin main

# 等同于
git fetch origin
git merge origin/main
```

---

## 实用技巧

### 1. 暂存工作进度

```bash
# 暂存当前工作
git stash

# 暂存包括未跟踪的文件
git stash -u

# 添加说明
git stash save "工作进度说明"

# 查看暂存列表
git stash list

# 恢复最近的暂存
git stash pop

# 恢复指定暂存
git stash apply stash@{0}

# 删除暂存
git stash drop stash@{0}

# 清空所有暂存
git stash clear
```

### 2. 标签管理

```bash
# 创建轻量标签
git tag v1.0.0

# 创建附注标签
git tag -a v1.0.0 -m "版本 1.0.0"

# 查看所有标签
git tag

# 查看标签详情
git show v1.0.0

# 推送标签到远程
git push origin v1.0.0

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

### 3. 查看特定提交

```bash
# 查看某次提交的详情
git show commit_hash

# 查看某个文件在某次提交的内容
git show commit_hash:filename.txt
```

### 4. 搜索

```bash
# 在代码中搜索
git grep "搜索内容"

# 在提交信息中搜索
git log --grep="关键词"

# 搜索添加或删除了特定内容的提交
git log -S "代码片段"
```

### 5. 忽略文件 (.gitignore)

创建 `.gitignore` 文件:
```
# 忽略所有 .log 文件
*.log

# 忽略 node_modules 目录
node_modules/

# 忽略所有 .env 文件
.env
.env.local

# 但不忽略 .env.example
!.env.example

# 忽略 build 目录
/build

# 忽略所有 .pdf 文件,除了 doc 目录下的
*.pdf
!doc/*.pdf
```

---

## 常见问题

### 1. 如何撤销已推送的提交?

```bash
# 方法 1: 使用 revert (推荐,创建新提交)
git revert commit_hash
git push origin main

# 方法 2: 强制推送(危险,会改写历史)
git reset --hard commit_hash
git push -f origin main
```

### 2. 如何解决合并冲突?

```bash
# 1. 尝试合并
git merge feature-branch

# 2. 如果有冲突,手动编辑冲突文件
# 查找 <<<<<<< HEAD 和 >>>>>>> feature-branch 标记

# 3. 解决冲突后,添加文件
git add conflicted-file.txt

# 4. 完成合并
git commit
```

### 3. 如何修改提交信息?

```bash
# 修改最后一次提交信息
git commit --amend -m "新的提交信息"

# 修改更早的提交(交互式变基)
git rebase -i HEAD~3
# 将要修改的提交前的 pick 改为 reword
```

### 4. 如何查看谁修改了某行代码?

```bash
# 查看文件每行的最后修改者
git blame filename.txt

# 查看特定行范围
git blame -L 10,20 filename.txt
```

### 5. 如何清理本地仓库?

```bash
# 删除未跟踪的文件(预览)
git clean -n

# 删除未跟踪的文件
git clean -f

# 删除未跟踪的文件和目录
git clean -fd

# 运行垃圾回收
git gc
```

---

## 最佳实践

### 提交信息规范

```
<type>(<scope>): <subject>

<body>

<footer>
```

**类型 (type):**
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式(不影响代码运行)
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建过程或辅助工具的变动

**示例:**
```
feat(auth): 添加用户登录功能

- 实现登录表单
- 添加 JWT 认证
- 集成第三方登录

Closes #123
```

### 分支命名规范

```
feature/功能名称    # 新功能
bugfix/问题描述     # bug 修复
hotfix/紧急修复     # 紧急修复
release/版本号      # 发布分支
```

### 工作流程建议

1. **开始新功能**
   ```bash
   git checkout -b feature/new-feature
   ```

2. **定期提交**
   ```bash
   git add .
   git commit -m "feat: 实现部分功能"
   ```

3. **保持同步**
   ```bash
   git fetch origin
   git rebase origin/main
   ```

4. **完成功能**
   ```bash
   git checkout main
   git merge feature/new-feature
   git push origin main
   ```

---

## 快速参考

### 常用命令速查

| 命令 | 说明 |
|------|------|
| `git init` | 初始化仓库 |
| `git clone <url>` | 克隆仓库 |
| `git status` | 查看状态 |
| `git add <file>` | 添加文件 |
| `git commit -m "msg"` | 提交 |
| `git push` | 推送 |
| `git pull` | 拉取 |
| `git branch` | 查看分支 |
| `git checkout <branch>` | 切换分支 |
| `git merge <branch>` | 合并分支 |
| `git log` | 查看历史 |
| `git diff` | 查看差异 |
| `git stash` | 暂存工作 |

---

## 学习资源

- 📚 [Pro Git 中文版](https://git-scm.com/book/zh/v2)
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN) - 交互式学习
- 📖 [Git 官方文档](https://git-scm.com/doc)
- 🎯 [GitHub Guides](https://guides.github.com/)

---

## 总结

Git 是一个强大的工具,需要时间和实践来掌握。建议:

1. ✅ 从基础命令开始(init, add, commit, push, pull)
2. ✅ 多练习分支操作
3. ✅ 养成频繁提交的习惯
4. ✅ 写清晰的提交信息
5. ✅ 不要害怕犯错,Git 几乎总能恢复

**记住:** Git 的设计初衷是保护你的代码,几乎所有操作都可以撤销!

---

*最后更新: 2026-01-12*
