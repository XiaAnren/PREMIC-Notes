## 历史记录被锁定

### 问题描述

`ipython`时遇到如下警告：
```bash
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/$CONDA_DEFAULT_ENV/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | History file was moved to /public/home/XiaAnRen/.ipython/$CONDA_DEFAULT_ENV/profile_default/history-corrupt.sqlite and a new file created.
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/$CONDA_DEFAULT_ENV/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | History file was moved to /public/home/XiaAnRen/.ipython/$CONDA_DEFAULT_ENV/profile_default/history-corrupt.sqlite and a new file created.
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/$CONDA_DEFAULT_ENV/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | Failed to load history too many times, history will not be saved.
```
可以正常运行但无法保存历史记录。

### 问题归因

**不明**，但似乎会与`HDF Error`同时出现。

### 解决方案（不推荐）

~~[禁用历史记录](https://www.cnblogs.com/dongnanfeng/p/19172624)~~：
```bash
cat > ~/.ipython/$CONDA_DEFAULT_ENV/profile_default/ipython_config.json << 'EOF'
{
  "HistoryManager": {
    "enabled": false
  }
}
EOF
```
