# PeMS08 Dataset Provenance and Information

## 1. Overview
- **Dataset Name**: PeMS08 (Caltrans Performance Measurement System District 8 - San Bernardino, California)
- **Primary Domain**: Freeway traffic flow volume forecasting & multi-variable spatiotemporal dynamics
- **Official Source**: California Department of Transportation (Caltrans) Performance Measurement System (PeMS)
- **Benchmark Provenance**: Standard ASTGCN / STGCN benchmark introduced by Guo et al. (AAAI 2019), distributed via TorchSpatiotemporal Benchmark Repository (`https://huggingface.co/datasets/TorchSpatiotemporal/ProcessedDatasets/resolve/main/traffic/pems08.zip`)
- **License / Usage Terms**: Caltrans Open Data Policy / Academic Research Public Domain (CC BY 4.0 / Public Research Use)
- **Role in TrafficPulse-X**: Optional traffic flow volume forecasting extension. **METR-LA remains the primary project dataset for historical speed forecasting.**

---

## 2. Dataset Characteristics
- **Spatial Coverage**: 170 loop detector stations across District 8 (San Bernardino freeway network: I-10, I-15, I-215, SR-60, SR-91, SR-210)
- **Temporal Coverage**: July 1, 2016 00:00:00 to August 31, 2016 23:55:00 (62 consecutive days)
- **Temporal Resolution**: 5-minute sampling interval (17,856 total timestamps)
- **Variables / Channels**:
  1. `flow`: Traffic volume in vehicles per 5-minute interval (`veh/5min`, values range: 0.0 to 1,147.0, mean: 230.68)
  2. `occupancy`: Detector loop occupancy ratio ($\in [0.0, 1.0]$, mean: 0.0651)
  3. `speed`: Vehicle velocity in miles per hour (`mph`, values range: 3.0 to 82.3, mean: 63.76)
- **Graph Topology**: Distance matrix in `distance.csv` recording physical road network distances between 170 sensor pairs (cost in meters).

---

## 3. Cryptographic Verification & File Checksums

| File | Size (Bytes) | SHA-256 Checksum |
| :--- | :--- | :--- |
| `pems08.zip` | 18,277,942 | `c137a96b8542050dab929470c311d8bae4f462981e6e569bf592f629695c9a88` |
| `pems08.npz` | 18,525,419 | `e1d03ce74e9fb79149e6e7c37c680214822c4a285baa2f42278cacabcd25c075` |
| `distance.csv` | 4,032 | `859d68e7583ed4e6016588eaaca494d3ece286dae361614c5c1833017f87eaa7` |

---

## 4. Preservation Invariants
1. **METR-LA Preservation**: Raw files in `data/raw/metr-la/` and processed pipelines in `data/processed/metr-la/` remain 100% frozen and unmodified.
2. **No Metric Relabeling**: METR-LA speed (`mph`) is never relabeled or masqueraded as traffic flow. Flow forecasting is strictly bound to PeMS08 channel 0 (`veh/5min`).
3. **Isolated Scalers**: The z-score normalization scaler for PeMS08 is computed exclusively on PeMS08 training split and never touches METR-LA `scaler.json`.
