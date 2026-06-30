## 安装`Miniforge`

### 安装流程

下载、安装、初始化：
```bash
>>> wget https://github.com/conda-forge/miniforge/releases/download/26.3.2-3/Miniforge3-26.3.2-3-Linux-x86_64.sh
>>> bash Miniforge3-26.3.2-3-Linux-x86_64.sh -b -p ~/miniforge3
>>> ~/miniforge3/bin/conda init bash
```

对于其中的`bash Miniforge3-26.3.2-3-Linux-x86_64.sh -b -p ~/miniforge3`：
- `-b`表示非交互批处理模式；
- `-p`指定安装路径（默认`~/miniforge3`，因此可省略）。
