# 灾害暴露度评估 | Disaster Exposure Assessment

将灾害危险区与资产/人口做空间叠加统计暴露量：栅格逐像元暴露量 = 危险区内价值之和（精确），并按危险等级分区汇总；矢量层面用 geopandas.sjoin 把资产点位与危险区多边形做空间连接，统计落入危险区的点位价值。危险区扩大（超集）时暴露量不减。

## 依赖

```bash
pip install numpy rasterio scipy geopandas shapely
```

## 使用方法

### 基本用法（合成数据，离线）

```bash
python geoskill-disaster-exposure-assessment.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 更多示例

```bash
python geoskill-disaster-exposure-assessment.py --bbox 116 39 117 40 --synthetic --output-dir ./out
python geoskill-disaster-exposure-assessment.py --input region.tif --threshold 1.0 --output-dir ./out
python geoskill-disaster-exposure-assessment.py --bbox 116 39 117 40 --breaks 0.3 1.0 2.0 --synthetic --output-dir ./out
python geoskill-disaster-exposure-assessment.py --bbox 121 31 122 32 --synthetic --quiet --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `exposed_mask.tif` | GeoTIFF | 暴露区掩膜（任一危险等级） |
| `hazard_zone.geojson` | GeoJSON | 矢量化危险区边界 |
| `exposure_stats.json` | JSON | 暴露价值/人口/分区统计/点位暴露 |

每次运行还会产出 `output-manifest.json`（运行清单，含输入/产物/QA 摘要）。

## 数据源 / Source

真实模式读取多波段 GeoTIFF（band1=危险强度、band2=资产价值、band3=人口）；合成模式离线生成场景（含资产点位）。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传任何用户数据。

## License

MIT
