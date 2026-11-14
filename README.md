# Surco: crop rotations read from space

Surco is a free, bilingual (English / Spanish) decision-support tool for the 2026 NASA Space Apps Challenge "Field Shift: Adapting Farms with NASA Data". It helps farmers explore crop rotations that protect soil and cope with changing heat, rain and water, using NASA Earth observations. It supports, and does not replace, farmer judgment and local agronomic advice.

**Live app:** https://YOUR-USER.github.io/surco/
*Youtube Video:** https://youtu.be/TTbOXBFK3-g
## What it does

1. **Your field's year**: monthly rain vs. reference evapotranspiration (ET0), MODIS NDVI with bare-soil months marked, root-zone soil wetness, days at or above 35 °C and frost days.
2. **How conditions are changing (2001–2025)**: Sen's slope per decade and a Mann–Kendall significance test for temperature, heat days, rain, ET0, soil wetness and frost.
3. **Rotation options**: every two-year rotation of 18 crops and cover crops suited to the place is simulated against each year of the NASA record. Three different directions (best balance, soil builder, water saver or income focus) are compared with the farmer's current rotation, ranked by the farmer's own priorities.
4. **Reasoning for every option**: why it was suggested, evidence from NASA data, possible benefits, information gaps, and what to check with local experts.
5. **Uncertainty**: 250 Monte Carlo runs give a 10–90% score range and how often each option comes out on top.
6. **What-if sliders**: rainfall change and warming re-run the whole analysis.
7. **Satellite layers**: NASA GIBS time-lapse of MODIS NDVI, SMAP L4 root-zone soil moisture, GPM IMERG rain rate, MODIS land-surface temperature, GRACE/GRACE-FO water storage, SRTM elevation and cropland share.
8. **Any location, three ways**: use the phone's GPS, type coordinates (with North/South and East/West buttons), or paste a Google Maps, Apple Maps or WhatsApp location link. Surco then pulls 25 years of NASA POWER data, 5 years of MODIS NDVI, SRTM terrain, an ERA5 cross-check and FAO GAEZ crop suitability live in the browser.
9. **Terrain from SRTM**: slope and altitude around the point set the erosion risk automatically.
10. **Second opinion from ERA5**: NASA POWER rain and temperature are compared with the Copernicus ERA5 reanalysis; the difference becomes the rain uncertainty used in every simulation.
11. **FAO GAEZ crop fit**: each option shows how suitable its crops are for rainfed farming at that point (1981–2010).
12. **Phone app**: installable from the browser (Progressive Web App); the sample fields work offline.

## NASA data

- **NASA POWER** daily point API v2.10 (Agroclimatology), 2001–2025: T2M_MAX, T2M_MIN, T2M, PRECTOTCORR, GWETROOT, GWETTOP, EVPTRNS, ALLSKY_SFC_SW_DWN, RH2M, WS2M.
- **MODIS Terra NDVI** 16-day L3 composites via **NASA GIBS** WMTS (`MODIS_Terra_L3_NDVI_16Day`), decoded with the GIBS `MODIS_L3_NDVI` colour map (median of a 3 × 3 pixel window), 2021–2025.
- **NASA GIBS map layers**: `SMAP_L4_Analyzed_Root_Zone_Soil_Moisture`, `IMERG_Precipitation_Rate`, `MODIS_Terra_Land_Surface_Temp_Day`, `GRACE_Tellus_Liquid_Water_Equivalent_Thickness_Mascon_CRI`, `SRTM_Color_Index`, `Agricultural_Lands_Croplands_2000`.

The three sample farms (rainfed maize in Morelos, Mexico; sorghum–maize in the Bajío, Mexico; irrigated corn in southwest Kansas, USA) embed NASA data downloaded on 5 November 2026, so the app also works offline.

- **SRTM** elevation (NASA/USGS, ~30 m) read through AWS Terrain Tiles (Terrarium encoding, zoom 12); median slope within ~120 m of the point (Horn method).
- **ERA5** reanalysis (Copernicus / ECMWF), daily precipitation and maximum temperature 2001–2025, through the Open-Meteo archive API.
- **FAO GAEZ v4** crop suitability index (1981–2010, rainfed, low and high input), through the FAO GAEZ ImageServer.

## Method

- Reference ET: Hargreaves–Samani (FAO-56 eq. 52) from daily max/min temperature and extraterrestrial radiation.
- Growing windows: months where rain ≥ 0.5 × ET0 (FAO agro-ecological zoning); in frost-limited places, the warm window starts when frost stops and nights exceed 8 °C.
- Crop model: monthly soil-water bucket (60 / 110 / 150 mm for sandy / loam / clay) for each historical year; crop need = Kc × ET0 (FAO-56); yield factor = 1 − Ky × (1 − water met) (FAO-33), times heat, frost and temperature-suitability factors.
- Scores (0–100): irrigation water, soil fertility (nitrogen credit + organic-matter returns), erosion protection (cover weighted by rain while exposed, scaled by slope), income (relative gross margin after ~40% input costs), pest-cycle break, stability in bad years (10th-percentile year vs. average), forage.

## Limits

Crop values are relative indices (maize = 1), not local prices. Soil water holding comes from texture, not a soil test. NASA POWER describes a ~50 km grid cell and MODIS a 250 m pixel; compare with local rain gauges and fields. Pests, diseases, labour, seed supply and equipment are covered only as written notes.

## Run it

Upload all files in this folder (`index.html`, `manifest.webmanifest`, `sw.js`, the icon PNGs, `README.md`, `LICENSE`) to a GitHub repository and turn on GitHub Pages (Settings → Pages → Branch `main`, folder `/`). The page needs no build step and no server. On a phone, open the site and choose “Install app” or “Add to Home screen”.

## Use of AI

This project was built with AI assistance. Claude (Anthropic) drafted most of the JavaScript (data aggregation, water-balance and scoring engine, interface), the bilingual interface text and documentation. The team reviewed, tested and validated the code, data and methods, and documented its own changes in the commit history. No AI-generated images, audio or video are used, and no NASA logos or branding elements are included.

## License

MIT, see `LICENSE`. Fonts: Zilla Slab, Public Sans, IBM Plex Mono (Google Fonts, SIL Open Font License).
