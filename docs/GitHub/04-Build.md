# 自动部署

## 初始化远程仓库

```bash
gh repo create [repo] --public
```

## 配置页面服务

```bash
gh api -X POST /repos/[user]/[repo]/pages -f build_type=workflow
```

## 初始化本地仓库

```bash
git init
git add .
git commit -m "init commit"
git branch -M main
```

## 关联远程仓库并推送

```bash
git remote add origin git@github.com:[user]/[repo].git
git push -u origin main
```
