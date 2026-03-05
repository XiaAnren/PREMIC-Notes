## 基础工作流

### 初始化本地仓库

```bash
git init
git add .
git commit -m "init commit"
git branch -M main
```

### 关联远程仓库并推送

```bash
git remote add origin git@github.com:[user]/[repo].git
git push -u origin main
```

### 后续推送

已经通过`-u`建立了追踪关系，因此可以直接使用`git push`和`git pull`：
```bash
git add .
git commit -m "[commit]"
git push
```
