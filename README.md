# Impact of Fog on LiDAR / Radar Sensors

Evaluates how fog degrades point-cloud density and reflected intensity across
**Velodyne**, **Blickfeld**, and **Radar** sensors, using paired clear/fog
captures from the RWU dataset.

## ⚠️ Data availability

The raw sensor data is **not included** in this repository. It was captured on
Hochschule Ravensburg-Weingarten campus premises and contains identifiable
pedestrians, so it is not published. See
[`docs/dataset_note.md`](docs/dataset_note.md) for the expected local folder
structure.

## Method

- Per-frame distance and intensity retention (fog / clear ratio), binned by range
- Global (pooled) and per-frame mean ± std retention curves
- Bird's-eye-view (BEV) visualizations to inspect spatial fog effects
- Point-count stability across a 500-frame sequence

## Evaluation Criteria

**Metrics used:** 1) Point Count 2) Intensity 3) Range reduction

**Distance retention** (per sensor, per distance bin):
1. Compute distance of every point: `sqrt(x² + y² + z²)`
2. Bin points into distance ranges (e.g. 0–0.5 m, 0.5–1 m, ...)
3. Per bin: `retention = fog points / clear points` → what % of points at this
   distance survive the fog
4. Repeat across all frames
5. **Global retention** = total fog points / total clear points, per bin
   → answers "how many points survive at this distance, across the whole sequence"
6. **Mean / variance retention** = retention computed per-frame per-bin, then
   averaged with standard deviation → answers "how stable is retention at this
   distance across frames" (not just the pooled average)

**Point count retention:**
`avg drop = (avg fog points − avg clear points) / avg clear points`

**Intensity retention:**
`avg drop = (avg fog intensity − avg clear intensity) / avg clear intensity`

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

**BEV comparisons — Velodyne, Blickfeld, Radar (clear vs. fog):**

<img width="1570" height="790" alt="image" src="https://github.com/user-attachments/assets/5d4afc56-0896-46d8-af30-3eef4058ab68" />
<img width="1574" height="790" alt="image" src="https://github.com/user-attachments/assets/d60ef0b5-3b60-419f-8329-005c5f243a6d" />
<img width="1589" height="817" alt="image" src="https://github.com/user-attachments/assets/61fa6c51-5421-43e9-9a90-0b95c95a8f4d" />

## Learnings

### Point count drop
| Sensor | Drop |
|---|---|
| Velodyne | ~17% (max drop) |
| Blickfeld | ~11% |
| Radar | <4% |

**Reason:** Fog absorbs and scatters energy, so Velodyne misses returned light
entirely on some beams → missing scan lines. Wet-ground reflections cause
small spikes in point count at 2–3 m.

### Intensity drop
| Sensor | Drop |
|---|---|
| Velodyne | ~4% (weak points get discarded — hard intensity threshold) |
| Blickfeld | ~70% (max drop — actively registers the low-intensity fog reflection cloud) |

**Blickfeld "fog wall":** severe backscattering — the sensitive MEMS mirror
picks up thousands of immediate reflections right in front of the lens,
producing a dense, low-intensity cloud blob near the sensor.

**Radar:** its longer wavelength means fog has very little effect overall —
the most fog-robust sensor of the three by a wide margin.

### Distance retention — ballpark numbers
- **Velodyne:** 250+ points preserved out to ~6.5 m
- **Blickfeld:** 50+ points preserved out to ~18.5 m
- **Radar:** points preserved past 25 m (sensitive sensor + artificial points, no real dropoff)

Overall ranking: **Radar > Blickfeld > Velodyne**

### Retention variance (stability across frames)
- **Velodyne:** variance spikes around ~10 m — a transition zone with a poor
  signal-to-noise ratio
- **Blickfeld:** variance rises due to sensor sensitivity
- **Radar:** variance rises due to multipath effects

### Point count retention (avg drop)
| Sensor | Avg drop | Notes |
|---|---|---|
| Velodyne | −17% | Discards points |
| Blickfeld | −11% | Keeps points |
| Radar | −3% | Dynamic noise, multipath |
