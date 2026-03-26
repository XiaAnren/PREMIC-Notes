## 安装`PyCINRAD`

### 问题描述

`PyCINRAD`没有在`conda`发布：
```bash
conda search cinrad
```

### 解决方案

使用`pip`进行安装：
```bash
pip install cinrad
```

### 问题描述

在安装过程中需要从源码进行编译（当前的`Python`版本是`3.12`，没有预编译好的`wheel`包），`pip`临时构建的环境中缺少`setup.py`脚本在编译前需要导入的包。

### 解决方案

使用`--no-build-isolation`参数使用当前环境中已经安装好的包，而不是创建一个临时的隔离环境：
```bash
pip install cinrad --no-build-isolation
```

### 问题描述

环境默认使用`icc`作为`C`编译器。

### 解决方案

改用`gcc`：
```bash
CC=gcc pip install cinrad --no-build-isolation
```

### 问题描述

`gcc`版本太老。

### 解决方案

使用`C99`标准进行编译（暂未尝试），即`--std=c99`或者`-std=gnu99`，后者在前者的基础上包含了一些扩展语法：
```bash
CFLAGS="-std=gnu99" CC=gcc pip install cinrad --no-build-isolation
```

使用`conda`安装现代版本的编译器（`defaults`版本有冲突，无法安装）：
```bash
conda install -c conda-forge compilers
```

### 总结

对于`python < 3.12`：
```bash
pip install cinrad
```

对于`python >= 3.12`：
```bash
conda install -c conda-forge compilers
pip install cinrad --no-build-isolation
```
