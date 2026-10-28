# MLVis 1K Dataset Specification & Attribution

## 1. Source & Citation
- **Source Repository:** [kdd21-MLVis](https://github.com/xeniaqian94/kdd21-MLVis)
- **Paper:** "Learning to Recommend Visualizations from Data", KDD'21 Research Track
- **Authors:** Xin Qian et al. (University of Maryland / Plotly research team)
- **Primary Data Origin:** Sampled 1,000 datasets and corresponding visualization configurations curated from the raw Plotly / VizML corpus (https://vizml.media.mit.edu).

## 2. Directory Structure & File Inventory
- `data/mlvis/raw/vis_all.csv`: Unified visualization catalog containing 6,915 visualization records across 999 unique visualization sessions (`vis_fid`).
- `data/mlvis/raw/datasets/`: 999 raw dataset CSV files corresponding to the visualization sessions.
- `data/mlvis/raw/dataset_index.json`: Explicit mapping between original dataset identifiers (e.g. `ALvinYu:500`) and Windows-safe sanitized filenames (e.g. `ALvinYu__500.csv`) to resolve filesystem constraints on colons (`:`).
- `data/mlvis/processed/`: Processed paired feature matrices, train/val/test splits, and scaler artifacts.

## 3. Visualization Records & Supported Types
In `vis_all.csv`, the distribution across visualization types is:
- `bar`: 1,791 records (25.9%)
- `scatter`: 1,669 records (24.1%)
- `scattergeo`: 1,136 records (16.4%)
- `box`: 337 records (4.9%)
- `scatter3d`: 35 records (0.5%)
- `heatmap`: 33 records (0.5%)
- `histogram`: 19 records (0.3%)
- `scattermapbox`: 15 records
- `surface`: 11 records
- `pie`: 8 records
- `table`: 5 records
- `candlestick`: 4 records
- `scattergl`: 3 records
- `contour`: 1 record

## 4. Preprocessing Assumptions & Leakage Safeguards
1. **Dataset-to-Visualization Pairing:** Each visualization record in `vis_all.csv` references its host dataset via source columns (`xsrc`, `ysrc`, `zsrc`, `valuessrc`) in the format `<author>:<dataset_num>:<column_id>`.
2. **Axis/Field Mapping:** `xsrc` and `ysrc` provide the ground-truth column assignments for x-axis and y-axis visual encodings.
3. **Data Quality & Filtering:** Records with missing chart types or datasets with single values/zero variance are handled gracefully. Datasets are grouped so that all visualizations belonging to a dataset remain strictly within the same split (Train, Validation, or Test) to eliminate cross-dataset data leakage.
4. **Session Independence:** The MLVis dataset contains dataset-level visualization configurations without user session streams; recommendations rely purely on dataset meta-features, visual task suitability, and field encoding assignments.
