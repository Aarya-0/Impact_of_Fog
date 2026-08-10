# Dataset Note

The raw point-cloud data used in this analysis (`CBuilding/`, `ParkingSlot/`)
is **not included** in this repository.

It was captured on the Hochschule Ravensburg-Weingarten campus and includes
identifiable pedestrians, so it cannot be published publicly under GDPR.

## Expected local structure

To run the notebook, keep the dataset locally in this layout relative to the
repo root:

    CBuilding/csv/
    ├── c_building_pedestrian_clear_anon/
    │   ├── velodyne/
    │   ├── blickfeld/
    │   └── radar/
    └── c_building_pedestrian_fog_anon/
        ├── velodyne/
        ├── blickfeld/
        └── radar/

Each sensor folder contains per-frame `.csv` files (`000000.csv`, `000001.csv`, ...)
with columns `x, y, z, intensity`.