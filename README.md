# EV Charging Deserts

# EV Charging Desert Discovery & Scoring Engine

A geospatial analysis tool built in Node.js to locate, evaluate, and rank optimal investment sites for commercial EV DC Fast Charging (DCFC) infrastructure across the United States. 

The engine cross-references real-time operational station data from the National Laboratory of the Rockies (NLR / AFDC) API against core transportation corridors to isolate geographic gaps and rank them by investment viability.

---

## Features

* **Live Infrastructure Ingestion:** Natively queries active public DC Fast Chargers using the live NLR developer endpoints.
* **Geospatial Proximity Extraction:** Leverages `@turf/turf` to run spatial exclusion buffers (35-mile radii) around existing chargers, isolating true infrastructure deserts.
* **Priority Matrix Scoring:** Evaluates gaps using a multi-variable investment optimization formula.
* **On-Demand Location Underwriting:** Features an interactive engine that dynamically scores any arbitrary latitude and longitude pair on a 0.0 to 10.0 scale.

---

## The Algorithmic Scoring Framework

Sites that pass the initial geographic exclusion filter are prioritized using the following weighted matrix:

$$\text{Score} = \alpha \cdot (\text{Traffic Volume}) + \beta \cdot (\text{Distance to Nearest DCFC}) - \gamma \cdot (\text{Grid Sparsity Penalty})$$

* **$\alpha$ (Traffic Weight):** Prioritizes corridors with high Annual Average Daily Traffic (AADT).
* **$\beta$ (Distance Weight):** Rewards sites further away from existing charging clusters to prevent market saturation.
* **$\gamma$ (Grid Sparsity Penalty):** Penalizes deep rural locations with constrained utility capacity where grid tie-in costs (transformers, substations) reduce project ROI.

---

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

1. Copy `.env_sample` to `.env`:

```bash
cp .env_sample .env
```

2. Edit `.env` and add your NLR API key:

```
API_KEY=your_actual_nlr_api_key_here
```

Get a free API key at [developer.nlr.gov](https://developer.nlr.gov).

3. (Optional) If needed, update the `API_URL` endpoint or fuel filter parameters in `index.js`.

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

- The project uses `@turf/turf` for spatial calculations and `dotenv` to load environment variables.
- The current highway network is synthetic and intended for prototyping only.
- The scoring engine is a simple heuristic and can be adapted with real traffic, grid, and demand data.
- **Never commit `.env` to version control** — it contains sensitive API keys. The `.env_sample` file is safe to commit as a template.

## License

This repository uses the `ISC` license configured in `package.json`.
