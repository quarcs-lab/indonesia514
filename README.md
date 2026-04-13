# Indonesia514

A Data Science Repository to Study Regional Development across **514 Districts** in Indonesia.

**Website:** [https://quarcs-lab.github.io/indonesia514/](https://quarcs-lab.github.io/indonesia514/)

## Project Status

This project is in **early development** (initial draft). Here is the current status of each component:

| Component | Status | Notes |
| --- | --- | --- |
| Website (`index.html`) | Draft | Bilingual SPA (English/Bahasa) with all sections |
| District boundaries (GeoJSON) | Complete | 514 district polygons in multiple formats |
| GDP data (`gdp/gdp.csv`) | Sample | 16-row sample; full 514-district data pending |
| GFCF data (`gfcf/gfcf.csv`) | Sample | 16-row sample; full 514-district data pending |
| Gov. Spending (`gs/gs.csv`) | Sample | 16-row sample; full 514-district data pending |
| Interactive apps (GeoExplorer) | Planned | Placeholder on website |
| Jupyter notebooks | Planned | Placeholder cards on website |
| GitHub Pages deployment | Pending | Enable in repository settings |

### Roadmap

- [ ] Replace sample CSVs with full 514-district datasets
- [ ] Create exploratory data analysis (EDA) notebook
- [ ] Create spatial analysis notebook
- [ ] Build GeoExplorer interactive app
- [ ] Add night-time lights and population data
- [ ] Add satellite embeddings data
- [ ] Deploy website via GitHub Pages

## Focus Areas

- **Regional GDP** — District-level Gross Domestic Product analysis
- **Investment (GFCF)** — Gross Fixed Capital Formation patterns
- **Government Spending** — Fiscal policy and public expenditure distribution
- **Multi-level analysis** — District > Province > Island aggregation

## Datasets

All datasets use `districtID` as the primary join key.

| Dataset | Description | Join Key |
| --- | --- | --- |
| `gdp/gdp.csv` | District GDP time series (2010–2022) | `districtID` |
| `gfcf/gfcf.csv` | Gross Fixed Capital Formation (2010–2022) | `districtID` |
| `gs/gs.csv` | Government Spending (2010–2022) | `districtID` |
| `maps/mapIdonesia514tp.geojson` | District boundaries (514 polygons) | `districtID` |

## Quick Start

```python
import pandas as pd

REPO_URL = "https://raw.githubusercontent.com/quarcs-lab/indonesia514/main"

df_gdp  = pd.read_csv(f"{REPO_URL}/gdp/gdp.csv")
df_gfcf = pd.read_csv(f"{REPO_URL}/gfcf/gfcf.csv")
df_gs   = pd.read_csv(f"{REPO_URL}/gs/gs.csv")

df = pd.merge(df_gdp, df_gfcf[['districtID', 'gfcf_2022']], on='districtID')
df = pd.merge(df, df_gs[['districtID', 'gs_2022']], on='districtID')
```

## Citation

Mendez, C., Abdulah, R., & Arvianto, B. (2026). Indonesia514: A Data Science Repository to Study Regional Development in Indonesia. GitHub. <https://github.com/quarcs-lab/indonesia514>

```bibtex
@misc{indonesia5142026,
  author = {Mendez, Carlos and Abdulah, Rusli and Arvianto, Bimo},
  title = {{Indonesia514}: A Data Science Repository to Study Regional Development in Indonesia},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/quarcs-lab/indonesia514}}
}
```

## License

MIT
