# OpenTraffic-vision

Public aggregate delivery repository for the released OpenTraffic task repositories.

## Delivery Contents

This repository includes:

1. aggregate release index in `index/public_release_index.json`
2. module-level delivery bundles in `modules/`
3. checksums in `checksums.txt`

## Included Repositories

1. `OpenTraffic-IQC`
2. `OpenTraffic-Water`
3. `OpenTraffic-Occluded`
4. `OpenTraffic-Snow`
5. `OpenTraffic-RLV`
6. `OpenTraffic-TLTV-YOLO`
7. `OpenTraffic-vehicle-flow-detection`
8. `OpenTraffic-Cars`
9. `OpenTraffic-NMV`

## Quick Start

1. Review `index/public_release_index.json`.
2. Select the target module under `modules/`.
3. Verify file integrity with the module-level `checksums.txt`.
4. Integrate the module-level `.so` and compare results with the provided examples.

See `USAGE.md` for the aggregate usage steps.
