# Working with Claude on Indonesia514

This guide helps you effectively collaborate with Claude (AI assistant) on the Indonesia514 project. Whether you're analyzing regional economic data, creating spatial visualizations, or extending the repository, these practices will help you get the best results.

## Project Overview

Indonesia514 is a data science repository for studying regional development across **514 districts (kabupaten/kota)** in Indonesia. The website is a single-page React 18 + Tailwind CSS application served from `index.html`.

## Project Structure

When working with Claude, reference these key directories:

- **`/gdp/`** — District GDP time series (2010–2022)
- **`/gfcf/`** — Gross Fixed Capital Formation time series (2010–2022)
- **`/gs/`** — Government Spending time series (2010–2022)
- **`/maps/`** — GeoJSON and GeoPackage boundary files for 514 districts
- **`/images/`** — Cover image and visual assets
- **`/index.html`** — The complete website (React SPA, ~2000 lines)

### Map Files

- `mapIdonesia514.geojson` — Full-resolution boundaries (49 MB)
- `mapIdonesia514tp.geojson` — Topology-preserving simplified (290 KB, use for web/Colab)
- `mapIdonesia514-opt.geojson` — Optimized version (6.1 MB)
- `mapIdonesia514.gpkg` / `mapIdonesia514tp.gpkg` — GeoPackage formats

**Note:** The filename uses "Idonesia" (typo preserved intentionally). Do not rename these files.

## Key Identifiers

**Always use `districtID` as the primary join key** when merging datasets. The geographic hierarchy is:

```
districtID → district (name)
provinceID → province (name)
islandID   → Island (name)
```

```python
# ✅ Correct
df_merged = pd.merge(df_gdp, df_gfcf, on='districtID', how='inner')

# ✅ Ensure integer type before merging with GeoJSON
gdf['districtID'] = gdf['districtID'].astype(int)
```

The 514 districts are organized across 34 provinces and 8 major islands: Sumatra, Java, Bali, Nusa Tenggara, Kalimantan, Sulawesi, Maluku, and Papua.

## Website Architecture

The website (`index.html`) is a **self-contained React 18 SPA** with no build step:

- **CDN dependencies:** React 18, ReactDOM 18, Babel Standalone, Tailwind CSS, Font Awesome
- **Bilingual:** English (`en`) + Bahasa Indonesia (`id`) via React Context API
- **Language toggle:** Stored in `localStorage` under key `indonesia514_lang`
- **Content registry:** `CONTENT_REGISTRY` array defines all notebooks, apps, and datasets
- **Translation keys:** `translations.en` and `translations.id` objects
- **Content i18n fields:** `title` / `title_id`, `description` / `description_id`

### When Editing the Website

- All changes go in `index.html` — there is no build system
- To add a new dataset card, add an entry to `CONTENT_REGISTRY`
- To add translations, add keys to both `translations.en` and `translations.id`
- The language code for Bahasa Indonesia is `id` (ISO 639-1), not `ind` or `ba`
- Test locally by opening `index.html` in a browser — no server needed
- All GitHub URLs use `quarcs-lab/indonesia514` on the `main` branch (not `master`)

## Quick Start Prompts

### Data Analysis
```
"Load the GDP and GFCF data for Indonesian districts and create a scatter plot
of GDP vs investment for 2022, colored by island."
```

### Spatial Analysis
```
"Create a choropleth map of district GDP in 2022 using the simplified GeoJSON
boundaries. Highlight the top 20 districts."
```

### Cross-Island Comparison
```
"Aggregate GDP, GFCF, and government spending by island for 2022. Create a
grouped bar chart comparing the three indicators across 8 islands."
```

## Common Tasks

### 1. Loading and Merging Data

**Prompt Template:**
```
"Load [dataset1] and [dataset2] from the repository, merge them on districtID,
and show me the first few rows with columns [specific columns]."
```

**Example:**
```
"Load GDP and government spending data, merge them, and calculate the ratio
of government spending to GDP for each district in 2022."
```

### 2. Spatial Visualization

**Prompt Template:**
```
"Create a choropleth map of [variable] using mapIdonesia514tp.geojson.
Use a [color scheme] colormap and add district boundaries."
```

**Example:**
```
"Create a choropleth map of gdp_2022 with a viridis colormap. Identify
the top 10 districts by GDP and label them on the map."
```

### 3. Time Series Analysis

**Prompt Template:**
```
"Analyze the temporal trend of [variable] from 2010 to 2022. Show
[visualization type] and calculate [specific statistics]."
```

**Example:**
```
"Compare GDP growth rates between Java and non-Java districts from 2010 to
2022. Create line plots by island and calculate average annual growth rates."
```

### 4. Inequality and Disparity Analysis

**Prompt Template:**
```
"Calculate [inequality metric] for [variable] across districts. Decompose
by [geographic level: province or island]."
```

**Example:**
```
"Calculate the Theil index for GDP per capita in 2022. Decompose inequality
into between-island and within-island components."
```

### 5. Adding a New Dataset

**Prompt Template:**
```
"I want to add [new dataset]. Help me:
1. Create the CSV with districtID as the join key
2. Add a README in the data folder
3. Add a dataset card to CONTENT_REGISTRY in index.html
4. Add translations for title and description in both en and id
5. Update the dataset table in the How to Use section
6. Update README.md"
```

## Best Practices for Prompts

### Be Specific About Data Sources
```
✅ "Use the GDP data at /gdp/gdp.csv and the simplified map at /maps/mapIdonesia514tp.geojson"
❌ "Use the data"
```

### Specify Output Format
```
✅ "Create a matplotlib figure with 2 subplots: (1) GDP distribution histogram
and (2) choropleth map of GDP by district"
❌ "Show me GDP"
```

### Request Validation Steps
```
✅ "Merge the datasets, verify the row count matches 514 districts,
and check for missing values in gdp_2022"
❌ "Merge the datasets"
```

### Ask for Interpretations
```
✅ "After creating the map, explain visible spatial patterns and suggest
potential factors driving inter-island disparities"
❌ "Create a map"
```

## Code Style Preferences

When writing code for this project:

- **Use pandas** for tabular data manipulation
- **Use geopandas** for spatial operations
- **Use matplotlib/seaborn** for static visualizations
- **Use plotly** for interactive charts
- **Use PySAL** for spatial statistics
- **Raw data URLs** should use: `https://raw.githubusercontent.com/quarcs-lab/indonesia514/main/`
- **GitHub blob URLs** should use: `https://github.com/quarcs-lab/indonesia514/blob/main/`

## Debugging and Troubleshooting

### Data Type Issues
```
"I'm getting a merge error with districtID. Check the data types in both
dataframes and convert to integers if needed before merging."
```

### Spatial Data Problems
```
"The GeoJSON isn't loading correctly. Verify the file path, check the CRS
(should be WGS84), and ensure districtID is properly formatted as integer."
```

### Website Issues
```
"The website isn't rendering. Open the browser developer console (F12),
check for JavaScript errors, and verify all CDN scripts are loading."
```

## Reproducibility

When working on analysis that should be reproducible:

```
"Create a complete workflow that:
1. Uses only raw data from the repository (provide GitHub raw URLs)
2. Includes all data processing steps
3. Sets random seeds for any stochastic operations
4. Generates publication-ready figures
5. Runs in Google Colab without local dependencies"
```

## Resources

- **Website**: [https://quarcs-lab.github.io/indonesia514/](https://quarcs-lab.github.io/indonesia514/)
- **Repository**: [https://github.com/quarcs-lab/indonesia514](https://github.com/quarcs-lab/indonesia514)
- **Reference project**: [DS4Bolivia](https://github.com/quarcs-lab/ds4bolivia) — the template this project is based on

---

**Pro Tip**: Start simple and iterate. Begin with basic data exploration, then progressively add complexity. Claude can help at every step, from initial data loading to publication-ready visualizations.
