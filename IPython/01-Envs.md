## 多环境冲突

### 问题描述

`ipython`时遇到如下警告：
```bash
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | History file was moved to /public/home/XiaAnRen/.ipython/profile_default/history-corrupt.sqlite and a new file created.
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | History file was moved to /public/home/XiaAnRen/.ipython/profile_default/history-corrupt.sqlite and a new file created.
[TerminalIPythonApp] ERROR | Failed to open SQLite history /public/home/XiaAnRen/.ipython/profile_default/history.sqlite (database is locked).
[TerminalIPythonApp] ERROR | Failed to load history too many times, history will not be saved.
```
可以正常运行但无法保存历史记录。

### 问题归因

多个环境共用同一个历史记录文件`~/.ipython/profile_default/history.sqlite`导致数据库被锁定。

### 解决方案

为指定环境创建配置目录：
```bash
mkdir ~/.ipython/$CONDA_DEFAULT_ENV
```

为指定环境创建激活脚本：
```bash
cat > $CONDA_PREFIX/etc/conda/activate.d/ipython_activate.sh << 'EOF'
export IPYTHONDIR_TEMP=$IPYTHONDIR
export IPYTHONDIR=~/.ipython/$CONDA_DEFAULT_ENV
EOF
```

为指定环境创建停用脚本：
```bash
cat > $CONDA_PREFIX/etc/conda/deactivate.d/ipython_deactivate.sh << 'EOF'
export IPYTHONDIR=$IPYTHONDIR_TEMP
unset IPYTHONDIR_TEMP
EOF
```
