## 覆盖错误的commit内容

### 覆盖本地的commit内容

```bash
git commit --amend -m "[commit]"
```

### 强制推送覆盖远端（如果原commit已推送）

未测试`--force`是否必需：
```bash
git push --force
```

### 立即删除本地的悬空推送

```bash
git reflog expire --expire=now --all
git gc --prune=now
```
