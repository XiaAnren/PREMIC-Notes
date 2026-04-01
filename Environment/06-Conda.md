## 更新`conda`

### 问题描述

旧版本的`conda`缺少`conda doctor --fix [--dry-run] [--verbose]`的功能，因此考虑进行更新。

但是`conda update -n base -c defaults conda`只更新了`ca-certificates`，并且`conda --version`和`conda -V`得到的仍然是旧版本号。

### 解决方案

指定版本号：
```bash
conda install -n base -c defaults conda=x.x.x
```

<details>
<summary>其他方案（未验证）</summary>

参考链接：[CSDN](https://blog.csdn.net/Mrfive555/article/details/130442044)、[GitHub](https://github.com/conda/conda/issues/12519)

加入参数`--repodata-fn=repodata.json`：
```bash
conda update -n base -c defaults conda --repodata-fn=repodata.json
```

`conda`默认使用`repodata.json`，后来改用了压缩格式`.bz2`或`.zst`。

因此旧版有时无法正确解析新格式，看不到最新的`conda`版本，需要指定参数显式读取未压缩的`json`文件。

</details>
