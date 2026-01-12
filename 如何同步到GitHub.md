# 如何将本地仓库与 GitHub 同步

## 当前状态 ✅

你的本地仓库已经完成以下设置:

- ✅ Git 仓库已初始化
- ✅ 文件已添加并提交
- ✅ 远程仓库已配置 (origin → <https://github.com/AxonKitty/DayOneCrypto_Axon.git>)
- ⚠️ 推送时遇到权限问题

## 问题原因

推送失败是因为 Git 使用的账户 (`TansensenBuilder`) 没有权限推送到 `AxonKitty/DayOneCrypto_Axon` 仓库。

---

## 解决方案

### 方案 1: 使用 Personal Access Token (推荐)

#### 步骤 1: 创建 GitHub Personal Access Token

1. 登录 GitHub (使用 AxonKitty 账户)
2. 点击右上角头像 → **Settings**
3. 左侧菜单最下方 → **Developer settings**
4. 点击 **Personal access tokens** → **Tokens (classic)**
5. 点击 **Generate new token** → **Generate new token (classic)**
6. 设置:
   - **Note**: `CryptoNote Repo Access`
   - **Expiration**: 选择有效期 (建议 90 days 或 No expiration)
   - **Select scopes**: 勾选 `repo` (完整仓库访问权限)
7. 点击 **Generate token**
8. **重要**: 复制生成的 token (只显示一次!)

#### 步骤 2: 使用 Token 推送

在终端运行:

```bash
cd /Users/jasontian/0-JasonsLand/CryptoNote

# 推送时会要求输入用户名和密码
git push -u origin main

# 输入:
# Username: AxonKitty
# Password: [粘贴你的 Personal Access Token]
```

#### 步骤 3: 保存凭证 (可选,避免每次输入)

```bash
# 配置 Git 记住凭证
git config --global credential.helper osxkeychain

# 下次推送时输入一次 token,之后会自动保存
```

---

### 方案 2: 使用 SSH 密钥 (更安全,长期推荐)

#### 步骤 1: 检查是否已有 SSH 密钥

```bash
ls -al ~/.ssh
# 查找 id_rsa.pub 或 id_ed25519.pub
```

#### 步骤 2: 生成新的 SSH 密钥 (如果没有)

```bash
# 生成 SSH 密钥
ssh-keygen -t ed25519 -C "your_email@example.com"

# 按提示操作:
# - 按 Enter 使用默认位置
# - 输入密码 (可选,建议设置)
# - 再次输入密码确认
```

#### 步骤 3: 添加 SSH 密钥到 ssh-agent

```bash
# 启动 ssh-agent
eval "$(ssh-agent -s)"

# 添加密钥到 ssh-agent
ssh-add ~/.ssh/id_ed25519
```

#### 步骤 4: 将 SSH 公钥添加到 GitHub

```bash
# 复制公钥到剪贴板
pbcopy < ~/.ssh/id_ed25519.pub
```

然后:

1. 登录 GitHub (AxonKitty 账户)
2. 右上角头像 → **Settings**
3. 左侧菜单 → **SSH and GPG keys**
4. 点击 **New SSH key**
5. 设置:
   - **Title**: `Mac - CryptoNote`
   - **Key**: 粘贴刚才复制的公钥
6. 点击 **Add SSH key**

#### 步骤 5: 修改远程仓库 URL 为 SSH

```bash
cd /Users/jasontian/0-JasonsLand/CryptoNote

# 查看当前远程仓库
git remote -v

# 修改为 SSH URL
git remote set-url origin git@github.com:AxonKitty/DayOneCrypto_Axon.git

# 验证修改
git remote -v
```

#### 步骤 6: 推送到 GitHub

```bash
# 首次推送
git push -u origin main
```

---

## 日常同步操作

设置完成后,日常使用这些命令:

### 推送本地更改到 GitHub

```bash
# 1. 查看修改状态
git status

# 2. 添加所有修改的文件
git add .

# 3. 提交更改
git commit -m "描述你的更改"

# 4. 推送到 GitHub
git push
```

### 从 GitHub 拉取最新更改

```bash
# 拉取并合并远程更改
git pull
```

### 一键同步脚本 (可选)

创建一个快捷脚本 `sync.sh`:

```bash
#!/bin/bash

echo "🔄 开始同步..."

# 添加所有更改
git add .

# 提交 (带时间戳)
git commit -m "Auto sync: $(date '+%Y-%m-%d %H:%M:%S')"

# 推送到远程
git push

echo "✅ 同步完成!"
```

使用方法:

```bash
# 赋予执行权限
chmod +x sync.sh

# 运行同步
./sync.sh
```

---

## 验证同步状态

```bash
# 查看远程仓库信息
git remote -v

# 查看本地和远程的差异
git status

# 查看提交历史
git log --oneline -5

# 查看远程分支
git branch -r
```

---

## 常见问题

### Q1: 推送时提示 "Updates were rejected"

**原因**: 远程仓库有本地没有的提交

**解决**:

```bash
# 先拉取远程更改
git pull --rebase origin main

# 然后推送
git push
```

### Q2: 如何撤销本地未推送的提交?

```bash
# 撤销最后一次提交,保留更改
git reset --soft HEAD^

# 撤销最后一次提交,不保留更改
git reset --hard HEAD^
```

### Q3: 如何查看远程仓库的最新状态?

```bash
# 获取远程更新(不合并)
git fetch origin

# 查看远程和本地的差异
git log origin/main..main  # 本地领先的提交
git log main..origin/main  # 远程领先的提交
```

### Q4: 如何强制推送? (谨慎使用!)

```bash
# 强制推送会覆盖远程历史,仅在确定时使用
git push -f origin main
```

---

## 推荐工作流程

### 每天开始工作前

```bash
# 1. 拉取最新代码
git pull

# 2. 查看状态
git status
```

### 工作过程中

```bash
# 频繁提交小的更改
git add .
git commit -m "feat: 添加新功能"
```

### 每天结束工作时

```bash
# 推送所有更改到 GitHub
git push
```

---

## 自动同步设置 (高级)

如果你想要自动同步,可以设置 Git hooks:

创建 `.git/hooks/post-commit` 文件:

```bash
#!/bin/bash
# 每次提交后自动推送
git push origin main &
```

赋予执行权限:

```bash
chmod +x .git/hooks/post-commit
```

**注意**: 自动推送可能不适合所有场景,建议手动控制推送时机。

---

## 下一步

1. ✅ 选择方案 1 (Token) 或方案 2 (SSH) 完成认证设置
2. ✅ 执行首次推送: `git push -u origin main`
3. ✅ 验证 GitHub 仓库是否已更新
4. ✅ 开始日常的 add → commit → push 工作流程

---

## 快速命令参考

```bash
# 查看状态
git status

# 添加文件
git add .

# 提交
git commit -m "message"

# 推送
git push

# 拉取
git pull

# 查看远程仓库
git remote -v

# 查看提交历史
git log --oneline
```

---

*创建时间: 2026-01-12*
*仓库: <https://github.com/AxonKitty/DayOneCrypto_Axon>*
