---
tags: []
created: 2024-06-04 20:58
updated: 2025-06-10 15:00
dg-publish:
---
## 在GitHub上创建一个新库
在 GitHub 页面中 new 一个仓库，仓库名建议和本地文件夹名称保持一致

## 初始化本地仓库
打开自己的项目文件夹，右键》Git Bash Here，可以快速定位到本地仓库。用命令初始化 Git 仓库

```
git init -b main
git add .
git commit -m "First commit"
```

## 添加远程仓库的 URL

```
git remote add origin <REMOTE_URL>
```

## 推送至远程仓库

git push origin -u mian

