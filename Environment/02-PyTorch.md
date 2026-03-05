## `PyTorch`与`wrf-python`冲突

### 问题描述

先`import torch`再`import wrf`时遇到如下问题：
```bash
ImportError: /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/python3.12/site-packages/torch/lib/libgomp-a34b3233.so.1: version `GOMP_5.0' not found (required by /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/python3.12/site-packages/wrf/_wrffortran.cpython-312-x86_64-linux-gnu.so)
```

### 问题排查

根据之前的经验检查conda内的动态链接库：
```bash
>>> l $CONDA_PREFIX/lib/libgomp*
    lrwxrwxrwx XiaAnRen premic  16 B  2025-12-20 21:54:00 /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/libgomp.so ⇒ libgomp.so.1.0.0
    lrwxrwxrwx XiaAnRen premic  16 B  2025-12-18 17:25:32 /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/libgomp.so.1 ⇒ libgomp.so.1.0.0
    .rwxr-xr-x XiaAnRen premic 1.5 MB 2025-12-09 13:05:03 /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/libgomp.so.1.0.0
```

随后尝试`LD_PRELOAD=$CONDA_PREFIX/lib/libgomp.so.1 python script.py`运行成功，因此采用与上次一样的解决方案。

### 解决方案

将预加载命令写入环境激活脚本：
```bash
echo "export LD_PRELOAD=$CONDA_PREFIX/lib/libgomp.so.1" >> $CONDA_PREFIX/etc/conda/activate.d/env_activate.sh
```
