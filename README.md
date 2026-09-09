# Vegetation & Deforestation Monitor

A web application that monitors forest loss using satellite imagery — draw or upload any area of interest and get an automated vegetation change report.

**[Live demo →](#)** *(add your Streamlit Cloud link here)*

---

## Overview

This project explores how freely available satellite imagery and machine-learning land-cover classification can be combined into a simple, self-serve tool for monitoring vegetation and forest loss over time.

The idea for this project grew out of my experience working with the Yorta Yorta community in Victoria, Australia — it made me want to build something that could make satellite-based environmental monitoring more accessible. The application itself is generic: it works on any area worldwide, and a sample dataset covering the public Yorta Yorta Country boundary is included so anyone can try it without their own data.

The app compares two time periods (2019–2020 vs. 2024–2025) using Sentinel-2 imagery, applies a forest mask derived from Google's Dynamic World land-cover classifier to filter out cropland, and highlights areas of significant vegetation decline. Results can be exported as a PDF report.

---

## Features

- **Flexible area of interest** — draw a polygon/rectangle directly on the map, or upload your own GeoJSON
- **Optional points of interest** — upload KML or CSV (lat/lon) files to mark places on the map
- **Forest mask (Google Dynamic World)** — adjustable tree-probability threshold to exclude cropland from the analysis, avoiding false positives from harvested fields
- **Vegetation & forest-loss layers** — NDVI visualisation and a dedicated loss layer (2019–2020 vs. 2024–2025), all toggleable on the map
- **PDF report export** — satellite basemap, vegetation layers, area outline and points of interest, composited into a downloadable report
- **Sample dataset included** — one-click loading of an example area (Yorta Yorta Country boundary) to test the app without preparing your own data

---

## How it works

1. Draw an area on the map (or upload a GeoJSON) — optionally load the included sample area instead
2. Optionally upload points of interest (KML/CSV)
3. Adjust the Dynamic World forest-mask threshold in the sidebar
4. Review the vegetation and forest-loss layers on the map
5. Export a PDF report

---

## Technical notes

- Sentinel-2 composites are built from a May–September window each year, to keep the comparison consistent across seasons (avoids comparing a leafy summer scene to a bare winter one)
- Cloud/shadow masking is done per-pixel via the Sentinel-2 SCL band
- The forest mask uses Google Dynamic World's per-pixel tree probability (averaged over the period), which is more robust than a simple NDVI threshold at distinguishing forest from very green cropland
- Map tiles and PDF report images are generated on demand via Earth Engine's tile service; Streamlit caching is used to avoid redundant recomputation for a given area/threshold
- Because Earth Engine's non-commercial tier has a compute quota, heavy or repeated use may occasionally hit a rate limit — this is expected and not a bug in the app

---

## Technologies

- Python
- Google Earth Engine (Sentinel-2, Dynamic World)
- Streamlit
- GeoPandas / Shapely
- Folium / streamlit-folium
- Pillow (PDF report map compositing)
- ReportLab (PDF generation)

---

## Running locally

pip install -r requirements.txt
streamlit run app/app.py

You'll need a Google Earth Engine service account and to configure its credentials as Streamlit secrets (st.secrets["earthengine"]) — see Earth Engine's service account documentation for setup: https://developers.google.com/earth-engine/guides/service_account

---

## Future improvements

- Broader default area (currently limited to a fixed application boundary for the demo)
- User-selectable comparison dates
- Additional environmental layers

---

## Disclaimer

This application is a proof of concept developed for educational and portfolio purposes. It visualises vegetation change derived from publicly available satellite imagery and does not assess cultural significance or determine the causes of environmental change.

---

## Author

Pierre Misrai
GIS · Remote Sensing · Political & Environmental Geography
