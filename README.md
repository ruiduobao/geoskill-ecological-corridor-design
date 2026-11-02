# Ecological Corridor Design (geoskill-ecological-corridor-design)

> Builds a resistance surface from habitat suitability, identifies ecological corridors with Dijkstra least-cost paths, and computes the PC landscape connectivity index. Outputs resistance-surface and corridor GeoTIFFs plus a parameters JSON.

---

## 1. Overview

Resistance surface = (1 − suitability) × 100 + 1; source–sink least-cost paths are computed on a 4-connected raster graph with scipy.sparse.csgraph.dijkstra (automatically avoiding high-resistance zones) and converted into corridor rasters using the buffer width; the PC connectivity index = sum of squared area shares of connected patches, measuring overall landscape connectivity (a single large patch > fragmented small patches). Use cases: connectivity analysis for ecological conservation redlines, wildlife migration corridor planning, and urban greenway route selection.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-ecological-corridor-design.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `resistance_surface.tif` | GeoTIFF (float32) | Resistance surface [1,101], EPSG:4326 |
| `corridor.tif` | GeoTIFF (float32) | Corridor raster (1 = corridor) |
| `corridor_params.json` | JSON | Source/sink pixels, path length, total cost, PC index |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/software versions) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
