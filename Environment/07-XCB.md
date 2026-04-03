## 安装`XCB`依赖项

### 问题描述

环境变量内存在`$DISPLAY`时运行与`matplotlib`有关的脚本会报错：
```bash
>>> python -c "import matplotlib.pyplot as plt; _ = plt.figure()"
    qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found.
    This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

    Available platform plugins are: eglfs, minimal, minimalegl, offscreen, vnc, webgl, xcb.

    Aborted (core dumped)
>>> conda list qt
    # packages in environment at $CONDA_PREFIX:
    #
    # Name                     Version          Build            Channel
    pyqt                       5.15.10          py311h6a678d5_0
    pyqt5-sip                  12.13.0          py311h5eee18b_0
    qt-main                    5.15.2           h53bd1ea_10
    qtconsole                  5.5.1            py311h06a4308_0
    qtpy                       2.4.1            py311h06a4308_0 
```

### 解决方案

推测是`qt-main`的`xcb`插件（`libqxcb.so`）依赖一些系统库（`libxcb-icccm`、`libxcb-image`、`libxcb-keysyms`、`libxcb-render-util`等），但系统没装或者版本不对：
```bash
>>> find "$CONDA_PREFIX" -name libqxcb.so
    $CONDA_PREFIX/plugins/platforms/libqxcb.so
>>> ldd "$CONDA_PREFIX/plugins/platforms/libqxcb.so" | grep "not found"
    libxcb-icccm.so.4 => not found
    libxcb-image.so.0 => not found
    libxcb-keysyms.so.1 => not found
    libxcb-render-util.so.0 => not found
```

因此重新安装：
```bash
>>> conda install -c conda-forge xcb-util-wm xcb-util-image xcb-util-keysyms xcb-util-renderutil
```

<details>
<summary>其他方案（未采用）</summary>

使用`AGG`后端（会完全禁止`plt.show()`等交互式功能，因此没有采用）：
```bash
>>> MPLBACKEND=Agg python -c "import matplotlib.pyplot as plt; _ = plt.figure()"
```

</details>
