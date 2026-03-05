## `PIL` ImportError

### 问题描述

`python main.py`时遇到如下问题：
```bash
Traceback (most recent call last):
  File "/data3/XiaAnRen/vscode/pytorch/xtest/swin/main.py", line 9, in <module>
    from models import build_model
  File "/data3/XiaAnRen/vscode/pytorch/xtest/swin/models/__init__.py", line 1, in <module>
    from .build import build_model
  File "/data3/XiaAnRen/vscode/pytorch/xtest/swin/models/build.py", line 8, in <module>
    from .swin_transformer_v2 import SwinTransformerV2
  File "/data3/XiaAnRen/vscode/pytorch/xtest/swin/models/swin_transformer_v2.py", line 12, in <module>
    from timm.layers import DropPath, to_2tuple, trunc_normal_
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/timm/__init__.py", line 2, in <module>
    from .layers import is_scriptable, is_exportable, set_scriptable, set_exportable
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/timm/layers/__init__.py", line 8, in <module>
    from .classifier import create_classifier, ClassifierHead, NormMlpClassifierHead, ClNormMlpClassifierHead
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/timm/layers/classifier.py", line 15, in <module>
    from .create_norm import get_norm_layer
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/timm/layers/create_norm.py", line 14, in <module>
    from torchvision.ops.misc import FrozenBatchNorm2d
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/torchvision/__init__.py", line 5, in <module>
    from torchvision import datasets
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/torchvision/datasets/__init__.py", line 1, in <module>
    from ._optical_flow import KittiFlow, Sintel, FlyingChairs, FlyingThings3D, HD1K
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/torchvision/datasets/_optical_flow.py", line 10, in <module>
    from PIL import Image
  File "/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/PIL/Image.py", line 88, in <module>
    from . import _imaging as core
ImportError: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by /public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/PIL/../../.././libLerc.so.4)
```

### 一些尝试

`export LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH`后可以正常运行，但使用不了`ls`命令：
```bash
ls: relocation error: /lib64/libacl.so.1: symbol getxattr, version ATTR_1.0 not defined in file libattr.so.1 with link time reference
```
`LD_PRELOAD=$CONDA_PREFIX/lib/libstdc++.so.6 python main.py`同样可行，但不够“一劳永逸”。

### 问题排查

检查实际运行时所调用的文件路径：
```bash
>>> LD_DEBUG=libs LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH python main.py 2>&1 | grep libstdc++
    392774: find library=libstdc++.so.6 [0]; searching
    392774: trying file=/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/libstdc++.so.6
    392774: calling init: /public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/libstdc++.so.6
>>> LD_DEBUG=libs python main.py 2>&1 | grep libstdc++
    393216: find library=libstdc++.so.6 [0]; searching
    393216: trying file=/public/software/mathlib/hdf5/intel/1.8.12/lib/libstdc++.so.6
    393216: trying file=/public/software/mathlib/jasper/intel/1.900.1/lib/libstdc++.so.6
    393216: trying file=/public/software/mathlib/libpng/intel/1.6.34/lib/libstdc++.so.6
    393216: trying file=/public/software/mathlib/netcdf/intel/4.4.0/lib/libstdc++.so.6
    393216: trying file=/public/software/mathlib/szip/intel/2.1.1/lib/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/compiler/lib/intel64_lin/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/mkl/lib/intel64_lin/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/compiler/lib/intel64/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/mpi/intel64/lib/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/mpi/mic/lib/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/ipp/lib/intel64/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/tbb/lib/intel64/gcc4.7/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/debugger_2017/iga/lib/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/debugger_2017/libipt/intel64/lib/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/daal/lib/intel64_lin/libstdc++.so.6
    393216: trying file=/public/software/compiler/intel/intel-compiler-2017.5.239/compilers_and_libraries_2017.5.239/linux/daal/../tbb/lib/intel64_lin/gcc4.4/libstdc++.so.6
    393216: trying file=/public/home/XiaAnRen/Software/flex/lib/libstdc++.so.6
    393216: trying file=/public/home/XiaAnRen/Software/fftw/lib/libstdc++.so.6
    393216: trying file=/data3/XiaAnRen/PALM/PALM/rrtmg/lib/libstdc++.so.6
    393216: trying file=/public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/torch/lib/libstdc++.so.6
    393216: trying file=/lib64/libstdc++.so.6
    393216: calling init: /lib64/libstdc++.so.6
    393216: /lib64/libstdc++.so.6: error: version lookup error: version `GLIBCXX_3.4.21' not found (required by /public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/PIL/../../.././libLerc.so.4) (fatal)
ImportError: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by /public/home/XiaAnRen/data3/anaconda3/envs/pytorch/lib/python3.10/site-packages/PIL/../../.././libLerc.so.4)
    393216: calling fini: /lib64/libstdc++.so.6 [0]
```
说明报错是因为调用了系统文件，想要正常运行则应当调用conda内的文件。

### 解决方案

将预加载命令写入环境激活脚本：
```bash
echo "export LD_PRELOAD=$CONDA_PREFIX/lib/libstdc++.so.6" >> $CONDA_PREFIX/etc/conda/activate.d/env_activate.sh
```
