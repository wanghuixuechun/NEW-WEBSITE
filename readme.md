# 使用Shell和Git搭建Github Pagesge个人网站

- GitHub Pages 提供了一种**免费托管静态网站**的方法，适用于个人博客、作品集、简历网站等。
- 本教程将使用 Shell 命令 + Git 从零开始搭建一个 GitHub 个人网站，并推送到 GitHub。

## 1. 使用 GitHub API 创建 GitHub Pages 仓库
### 1.1 生成GitHub个人访问令牌（PAT）

GitHub 需要身份验证才能创建仓库，因此你需要一个 GitHub 个人访问令牌（Personal Access Token, PAT）：
  - 进入 [GitHub 个人访问令牌管理页面](https://github.com/settings/tokens)
  - 点击 "**Generate new token (classic)**"
  - 勾选 "**repo**" 权限（允许创建仓库）
![示例](image.png)
  - 生成令牌后，**复制并保存**（只会显示一次）

### 1.2 通过 GitHub API 创建仓库

使用 `crul` 通过GitHub API 自动创建一个 GitHub Pages 仓库：
```sh
# 1️⃣ 设置 GitHub 变量（替换 `your-username` 和 `your-github-token`）
GITHUB_USERNAME="your-username"
GITHUB_TOKEN="your-github-token"
REPO_NAME="${GITHUB_USERNAME}.github.io"

# 2️⃣ 使用 GitHub API 创建仓库
curl -u "$GITHUB_USERNAME:$GITHUB_TOKEN" \
     -X POST \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/user/repos \
     -d "{\"name\":\"$REPO_NAME\", \"private\":false}"
```
📌 成功后，你可以访问 `https://github.com/your-username/your-username.github.io` 看到你的仓库！

## 2. 在本地创建网站文件
### 2.2 本地创建网站目录
  ```sh
# 进入工作目录（可选）
cd ~

# 创建 GitHub Pages 站点目录
mkdir my_website && cd my_website

# 初始化 Git 仓库
git init
  ```
### 2.2 创建 `index.html`（网站主页）

```sh
cat <<EOF > index.html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的 GitHub 个人网站</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 50px; }
        h1 { color: #333; }
    </style>
</head>
<body>
    <h1>欢迎来到我的 GitHub 个人网站</h1>
    <p>这是一个托管在 GitHub Pages 上的静态网站！</p>
</body>
</html>
EOF
```
📌 这个 `index.html` 将成为你的网站首页。

### 2.3 关联GitHub远程仓库

```sh
git remote add origin https://github.com/$GITHUB_USERNAME/$REPO_NAME.git
```

## 3. 提交代码到GitHub
### 3.1 提交并推送代码

```sh
# 1️⃣ 添加所有文件
git add .

# 2️⃣ 提交文件
git commit -m "Initial commit - GitHub Pages site"

# 3️⃣ 推送到 GitHub
git branch -M main  # 确保当前分支是 main
git push -u origin main
```
📌 此时，你的代码已经成功推送到 GitHub 🎉。

## 4. 启动GitHub Pages
### 4.1 使用GitHub API 启用 GitHub Pages

```sh
curl -u "$GITHUB_USERNAME:$GITHUB_TOKEN" \
     -X POST \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/repos/$GITHUB_USERNAME/$REPO_NAME/pages \
     -d '{"source":{"branch":"main","path":"/"}}'
```
✅ 几分钟后，你可以访问 `https://your-username.github.io/ `看到你的网站！