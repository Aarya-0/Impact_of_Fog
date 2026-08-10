# Impact of Fog on LiDAR / Radar Sensors

Evaluates how fog degrades point-cloud density and reflected intensity across
**Velodyne**, **Blickfeld**, and **Radar** sensors, using paired clear/fog
captures from the RWU dataset.

## ⚠️ Data availability

The raw sensor data is **not included** in this repository. It was captured on
Hochschule Ravensburg-Weingarten campus premises and contains identifiable
pedestrians, so it cannot be published under GDPR. See
[`docs/dataset_note.md`](docs/dataset_note.md) for the expected local folder
structure.

## Method

- Per-frame distance and intensity retention (fog / clear ratio), binned by range
- Global (pooled) and per-frame mean ± std retention curves
- Bird's-eye-view (BEV) visualizations to inspect spatial fog effects
- Point-count stability across a 500-frame sequence

## Repo structure

    RWUDataset/
    ├── README.md
    ├── requirements.txt
    ├── .gitignore
    ├── notebooks/
    │   ├── final.ipynb            # main analysis notebook
    │   ├── HowTORWUDataset.ipynb
    │   └── HowTORWUDataset.py
    ├── results/                    # exported figures (not raw data)
    └── docs/
        └── dataset_note.md

## Usage

    pip install -r requirements.txt

Then open `notebooks/final.ipynb` and run top to bottom (dataset must be
present locally — see `docs/dataset_note.md`).

## Key results

_Add 2-3 exported figures from `results/` here once the notebook has been run._