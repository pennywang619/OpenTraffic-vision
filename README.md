<div align="right">

[**English**](README.md) | [中文](README_ZH.md)

</div>

<div align="center">

![OpenTraffic Perception Logo](LOGO2x.png)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=for-the-badge)](#license)
[![Python](https://img.shields.io/badge/Python-3.13+-green.svg?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Linux%20x86__64-orange.svg?style=for-the-badge&logo=linux&logoColor=white)]()
[![GitHub](https://img.shields.io/badge/GitHub-OpenHelix--Team-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/OpenHelix-Team)
[![Hugging Face](https://img.shields.io/badge/HuggingFace-Models-fcd022?style=for-the-badge&logo=huggingface&logoColor=white)](https://huggingface.co/)
[![Social](https://img.shields.io/badge/Social-Media-1DA1F2?style=for-the-badge&logo=x&logoColor=white)](#social-media)

</div>

### OpenTraffic Perception System

An open-source traffic perception engine for urban intersections, designed for perception-driven TSC pipelines. It supports multi-stream video access, vehicle recognition, world-coordinate output, world-speed estimation, lane-level result generation, and radar-based validation.

<br/>

![Perception System Framework Figure](框架图-.jpg)

> **Paper:** https://arxiv.org/abs/XXXX.XXXXX  
> **Hugging Face:** https://huggingface.co/  
> **GitHub:** https://github.com/OpenHelix-Team  
> **Social Media:** X / WeChat / GitHub Discussions

<br/>

## News

- **[2026/05/27]** Reorganized perception README using a unified English-first repository presentation style.
- **[2026/05/15]** Initial public perception package released for intersection video-to-world-state generation.

## TODO List

- [x] Vehicle recognition pipeline
- [x] World coordinate output
- [x] World speed output
- [x] Lane-level structured output
- [x] Redis stream publishing
- [x] Radar-reference validation flow
- [ ] Release public pretrained weights
- [ ] Add more intersection templates
- [ ] Add visualization dashboard
- [ ] Publish paper and benchmark page

## Table of Contents

- [Framework Figure](#framework-figure)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Functional Overview](#functional-overview)
- [Performance](#performance)
- [Citation](#citation)
- [License](#license)
- [Paper](#paper)
- [Hugging Face](#hugging-face)
- [Social Media](#social-media)
- [News](#news)
- [TODO List](#todo-list)
- [Directory](#directory)

## Paper

- Title: `OpenTraffic Perception System for Perception-Driven TSC`
- Status: `In preparation / to be released`
- Placeholder link: `https://arxiv.org/abs/XXXX.XXXXX`

## Hugging Face

- Model hub: `https://huggingface.co/`
- Planned content:
  - packaged perception models
  - example outputs
  - benchmark samples

## Social Media

- GitHub Organization: `https://github.com/OpenHelix-Team`
- X / Twitter: `To be announced`
- WeChat / Community group: `To be announced`

## Framework Figure

The current system-level framework figure used for repository presentation:

## Directory

```text
opentraffic-perception-engine-main/
├── README.md
├── README_ZH.md
├── figure/
│   ├── LOGO.png
│   ├── LOGO2x.png
│   ├── Framework.png
│   └── Framework_EN.png
├── 框架图-.jpg
└── opentraffic-TIR/
    ├── drivers/
    ├── recognizer/
    ├── input/
    ├── reference_radar/
    ├── redis-7.4.0/
    ├── run_local.sh
    ├── stop_local.sh
    └── requirements.txt
```

## Quick Start

### Requirements

- OS: `Linux x86_64`
- Python: `3.13+`
- NVIDIA Driver: `>= 560`
- CUDA runtime environment: `recommended for packaged GPU deployment`

### Installation

```bash
git clone <your-repository-url>
cd opentraffic-perception-engine-main/opentraffic-TIR
```

If using the source-style environment:

```bash
pip install -r requirements.txt
```

If using the packaged delivery environment:

```bash
bash run_local.sh
```

### Running

1. Prepare the video source through RTSP or local file.
2. Prepare local Redis.
3. Configure `drivers/config.json`.
4. Start the system:

```bash
bash run_local.sh
```

5. Quit with:

```bash
q
```

## Configuration

Edit `opentraffic-TIR/drivers/config.json`.


### Example

```json
{
  "intersection": {
    "id": "HHL_QHDD",
    "cameras": [
      {
        "id": "HHL_QHDD_S",
        "rtsp_url": "./input/45_0429.mp4",
        "window": [0, 0, 1920, 1080],
        "H": [[...], [...], [...]],
        "H_inv": [[...], [...], [...]]
      }
    ]
  },
  "jsonlOutputDir": "./control_group_fullspeed_jsonl",
  "radarReferenceJsonl": "./reference_radar/HHL_QHDD_S/HHL_QHDD_S_shard_00000.jsonl",
  "vehicleMatchMaxDist": 10.0,
  "localRedisConfig": {
    "host": "127.0.0.1",
    "port": 6379
  }
}
```

## Functional Overview

### 1. Vehicle Recognition

- Supports vehicle detection and multi-object tracking from intersection video.
- Works with RTSP streams or local video files.
- Generates structured target IDs for downstream traffic-state output.

### 2. World Coordinate Output

- Projects image-space detections into world-space coordinates.
- Uses calibrated homography matrices for pixel-to-world mapping.
- Produces radar-style coordinate outputs suitable for downstream integration.

### 3. World Speed Estimation

- Estimates object speed in world coordinates.
- Outputs structured speed fields for each tracked vehicle.
- Designed for traffic-state interpretation and validation against radar reference.

### 4. Lane Line Detection and Lane-Level Output

- Supports lane-level attribution for perceived vehicles.
- Associates vehicle targets with lane-level semantics.
- Produces lane-aware structured results for control-system consumption.

## Performance

Historical full-set speed JSONL vs. raw radar JSONL:

### Field Accuracy Comparison

| Field / Metric | Mean | Median (p50) | p75 | p90 | Notes |
|---|---:|---:|---:|---:|---|
| `timestamp_ms` error (ms) | `25.04` | `26.5` | `37` | `46` | Time delta between the video frame and the nearest radar frame |
| `vehicles[].center` error (m) | `2.5132` | `1.6251` | `3.1987` | `6.7021` | Spatial position error of matched targets |
| `vehicles[].speed_scalar` error (m/s) | `1.6727` | `0.3176` | `1.7741` | `5.0061` | Speed scalar error of matched targets |

### Field Consistency

| Field | Comparison Method | Consistency / Result |
|---|---|---:|
| `intersection_id` | Exact match rate | `100%` |
| `camera_id` | Exact match rate | `100%` |
| `coordinate_space` | Exact match rate | `100%` |
| `timestamp_ms` | Alignment rate within 200 ms | `100%` |
| `vehicles[].lane` | Exact match rate against radar | `100%` |

## Citation

If this project is useful in your work, please cite:

```bibtex
@article{opentraffic_perception_2026,
  title   = {OpenTraffic Perception System for Perception-Driven TSC},
  author  = {OpenTraffic Team},
  journal = {arXiv preprint},
  year    = {2026}
}
```

## License

This README is prepared in an Apache-2.0 style release template for repository presentation consistency.

Intended public release license:

```text
Apache License
Version 2.0, January 2004
http://www.apache.org/licenses/
```

Repository note:

- If this repository is publicly released under Apache-2.0, please add a root-level `LICENSE` file before publishing.
- The current packaged perception submodule may still contain other local license text and should be aligned before external release.

---

<div align="center">
  <b>OpenHelix-Team Style Repository Presentation</b><br/>
  English-first homepage with Chinese switch entry for unified GitHub display.
</div>
