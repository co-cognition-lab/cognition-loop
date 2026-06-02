# 初始 Push 手把手教程

> 一次性操作：将本地 `cognition-loop/` 目录推送到 GitHub
> 完成后通知 LobsterAI，后续由 LobsterAI 自行维护 push

---

## 前置确认

- [ ] P0 已在 `co-cognition-lab` org 下创建了空的 `cognition-loop` repo
- [ ] Repo 创建时**没有**勾选 "Add a README file"（如果有 README，需要先处理冲突）

---

## 步骤（在 PowerShell 中逐条执行）

### 1. 进入项目目录

```powershell
cd "D:\文档\LobsterProject\LiteratureHistoryPhilosophy\cognition-loop"
```

### 2. 初始化 Git

```powershell
git init
```

### 3. 添加远程仓库

```powershell
git remote add origin https://github.com/co-cognition-lab/cognition-loop.git
```

### 4. 暂存所有文件

```powershell
git add .
```

### 5. 提交

```powershell
git commit -m "Initial commit: cognition-loop Phase 1 skeleton"
```

### 6. 推送到 GitHub

```powershell
git branch -M main
git push -u origin main
```

---

## 如果第 6 步要求登录

GitHub 不再支持密码登录。选择以下方式之一：

### A. 用 GitHub CLI（推荐，最简单）

```powershell
gh auth login
# 选择 GitHub.com → HTTPS → Login with a web browser
gh auth setup-git
git push -u origin main
```

### B. 用 Personal Access Token

1. 打开 https://github.com/settings/tokens
2. Generate new token (classic) → 勾选 `repo` 权限
3. 复制 token
4. push 时用户名填 GitHub ID，密码粘贴 token

---

## 验证

推送成功后，打开浏览器确认：

```
https://github.com/co-cognition-lab/cognition-loop
```

应该能看到 LICENSE、README.md、seed-events/ 等文件。

---

## 完成后

回复 LobsterAI："已 push，后续维护交给你"——我再开始 Stage 2。
