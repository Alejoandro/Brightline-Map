# Brightline Florida Economic Impact Map

## Contents

```
brightline_map_package/
├── build_map.py               # Run this to regenerate the map
├── brightline_florida_map.html # Output — open in any browser
├── README.md
└── data/
    ├── corridor.json          # Corridor tract GeoJSON (polygon-edge distances)
    ├── map_data.json          # Stations, outcome labels
    ├── emp_did.pkl            # Employment DiD results (corrected, all buffers)
    ├── tract_did.pkl          # Business & housing DiD results (tract-interpolated)
    ├── tracts_exposure.csv    # Exposure scores per tract
    └── tracts_final.geojson   # Full tract geometries with corrected distances
```

## Usage

The map HTML is already built. Just open it:

```
open brightline_florida_map.html      # Mac
start brightline_florida_map.html     # Windows
xdg-open brightline_florida_map.html  # Linux
```

To rebuild after changing DiD estimates:

```bash
pip install geopandas pandas numpy
python build_map.py
```

## Map Features

- Toggle treatment radius: 0.5 / 1.0 / 2.0 miles
- Toggle economic variable: Employment, Payroll, Establishments,
  Business Employment, Housing Prices
- Click any Brightline station dot → DiD results popup
  (updates live when you change radius or variable)
- Hover any treatment tract → tract ID and predicted effect tooltip
- Blue = negative effect, Red = positive effect
- Station-containing tracts (dist = 0) shown at full effect intensity

## Methodology Notes

- Treatment assignment: polygon-edge distance (station point to tract polygon boundary)
- Control group: PSM-matched comparison city tracts (Jacksonville, Hillsborough,
  Sarasota, Alachua, Leon, Lee, Collier counties)
- Matching: 1:3 nearest-neighbor within employment trend tertile, 0.2 SD caliper
- DiD: two-way fixed effects (unit + year), weighted by PSM weights, clustered SEs
- Business/housing: ZIP-level data interpolated to tract level using
  employment-density weights (business) and population weights (housing)
- Exposure scores: fraction of tract area within each buffer ring
