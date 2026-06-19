# EV Charging Deserts

A small project that identifies high-priority highway corridors lacking DC Fast Charge infrastructure and writes the top 100 candidate locations to `top_100_charging_deserts.csv`.

## What it does

- Fetches existing operational public DC Fast Chargers from the NLR/AFDC API
- Generates a synthetic highway corridor dataset for major U.S. interstate proxies
- Calculates distance-based charging desert scores using a custom priority formula
- Produces a CSV of the top 100 sites that are farthest from active chargers and still receive corridor traffic
- Includes a simple interactive scoring engine for evaluating investment potential at any latitude/longitude

## Requirements

- Node.js 18 or newer
- npm

## Install

```bash
npm install
```

## Configure

1. In `index.js`, set `API_KEY` to a valid NLR API key:

```js
const API_KEY = 'DEMO_KEY';
```

2. If needed, update the `API_URL` endpoint or fuel filter parameters.

## Run

```bash
node index.js
```

This will:

- fetch station data from the API
- generate the synthetic highway network
- compute the top 100 charging deserts
- write `top_100_charging_deserts.csv`
- print example investment scores for sample coordinates

## Output

- `top_100_charging_deserts.csv` — top 100 candidate locations with:
  - longitude, latitude
  - corridor name
  - AADT proxy value
  - distance to nearest DC Fast Charger
  - computed investment score

## Notes

- The project uses `@turf/turf` for spatial calculations.
- The current highway network is synthetic and intended for prototyping only.
- The scoring engine is a simple heuristic and can be adapted with real traffic, grid, and demand data.

## License

This repository uses the `ISC` license configured in `package.json`.
