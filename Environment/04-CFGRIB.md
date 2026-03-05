## 安装`CFGRIB`

### 安装`CFGRIB`

直接安装会与`Xarray`冲突导致不可用，因此需要从同一来源进行安装：
```bash
pip uninstall xarray
conda install -c conda-forge xarray cfgrib
```

### 附录：使用`CFGRIB`作为`Xarray`的`GRIB`读取引擎

普通`GRIB`文件直接读取：
```python
import xarray as xr

xr.open_dataset(filename, engine="cfgrib")
```

`GFS`的`GRIB2`文件需要额外指定：
```python
import xarray as xr

xr.open_dataset(
  "2026020400_fh.0000_tl.press_gr.0p5deg.grib2",
  engine="cfgrib",
  backend_kwargs={
    "filter_by_keys": {"typeOfLevel": "surface", "stepType": "accum"},
  },
)
```
