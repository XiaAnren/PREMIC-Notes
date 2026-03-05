## `wrf-python` ImportWarning

### 问题描述

`import wrf`时遇到如下问题：
```bash
/public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6/lib/python3.12/site-packages/wrf/__init__.py:3: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources
```

### 问题排查

`wrf-python`内部导入了`pkg_resources`，而`pkg_resources`在`setuptools`中已经标记为弃用，因此发出警告。

检查`setuptools`的版本：
```bash
>>> conda list setuptools
    # packages in environment at /public/home/XiaAnRen/data3/anaconda3/envs/pytorch2.6:
    #
    # Name                     Version          Build            Channel
    setuptools                 80.9.0           py312h06a4308_0
```

### 解决方案

降级`setuptools`：
```bash
conda install 'setuptools<80'
```
