---
name: geoskill-ecological-corridor-design
description: '由栖息地适宜性构建阻力面，Dijkstra 最小成本路径识别生态廊道，并计算 PC 景观连通性指数。Designs ecological corridors with least-cost paths and connectivity indices. 输出阻力面与廊道 GeoTIFF + 参数 JSON。'
---

# 生态廊道设计 | Ecological Corridor Design

The resistance surface is computed as (1 − suitability) × 100 + 1; source–sink least-cost paths are solved on a 4-connected raster graph with scipy.sparse.csgraph.dijkstra (automatically bypassing high-resistance bands), and the corridor raster is generated according to the buffer width; the PC connectivity index is the sum of squared area proportions of connected patches, measuring overall landscape connectivity (a single large patch > fragmented small patches).

Use cases: connectivity analysis of ecological conservation red lines, wildlife migration corridor planning, and urban greenway route selection.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy geopandas shapely
```

## Usage / 使用方法

### Example 1: synthetic two-patch + corridor scenario

```bash
python geoskill-ecological-corridor-design.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./output
```

### Example 2: real suitability raster

```bash
python geoskill-ecological-corridor-design.py --input suitability.tif --output-dir ./real
```

### Example 3: wider corridor buffer

```bash
python geoskill-ecological-corridor-design.py --bbox 116 39 117 40 --synthetic --buffer 4 --output-dir ./wide
```

### Example 4: adjusting the PC threshold

```bash
python geoskill-ecological-corridor-design.py --bbox 121 31 122 32 --synthetic --pc-threshold 0.6 --output-dir ./pc06
```

### Example 5: silent batch run

```bash
python geoskill-ecological-corridor-design.py --bbox 113 23 114 24 --synthetic --quiet --output-dir ./batch
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `resistance_surface.tif` | GeoTIFF (float32) | Resistance surface [1,101], EPSG:4326 |
| `corridor.tif` | GeoTIFF (float32) | Corridor raster (1 = corridor) |
| `corridor_params.json` | JSON | Source/sink pixels, path length, total cost, PC index |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/software versions) |

## Data Source / 数据源 / Source

Local GeoTIFF (habitat suitability, optional); least-cost path and the PC index are published landscape-ecology methods; synthetic mode generates a two-patch scenario locally with no external data sources.

## Privacy / 隐私声明 / Privacy

- Runs fully offline by default and makes no network requests
- `--synthetic` mode reads no external data
- All computation is done locally; user data is never uploaded

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
