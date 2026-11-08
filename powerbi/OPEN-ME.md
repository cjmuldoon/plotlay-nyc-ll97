# Opening the Power BI report

`NYCLocalLaw97.pbip` is a Power BI Project: the report (`NYCLocalLaw97.Report`, PBIR format) and the semantic model (`NYCLocalLaw97.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). Projects and the PBIR report format are standard in current versions. If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is usually shared as `\\Mac\Home`.

## Open it

1. In Windows, go to the repo's `powerbi` folder and double-click `NYCLocalLaw97.pbip`, or in Desktop use **File, Open report, Browse reports** with the file type set to *Power BI project files (\*.pbip)*.
2. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-nyc-ll97/main/data/`). If Desktop asks how to connect, choose **Anonymous** and then **Connect**.
   - To read the files from disk instead, go to **Home, Transform data, Edit parameters** and set `DataFolder` to the full path of the repo's `data` folder, **ending with a backslash** (for example `\\Mac\Home\plotlay-nyc-ll97\data\`). Then select **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** (all the sources are public open data).
3. After the refresh, check page 1. With no period picked the measures read 2024 to 2029: Properties over the limit should match the README's 2024 to 2029 figure. Pick **2030 to 2034** on the Period slicer: the likely penalty a year should match the README's 2030 to 2034 figure. If both match, everything is joined correctly.

## The model

- **Properties**: one row per kept property (`data/tables/properties.csv`), with both periods' emissions, limits and likely penalties, its pathway and data check. All the measures live here and count only `in_totals` rows, except `Penalty, every record` (the sensitivity page).
- **Districts**: the 59 community districts (`community_districts.csv`), joined to Properties by `cd`.
- **Period**: 2024 to 2029 and 2030 to 2034, for the slicer. The measures read the picked period's columns (2024 to 2029 when none is picked).
- Measures: `Properties`, `Over the limit`, `Share over`, `Penalty a year`, `Tonnes over`, `Emissions`, `Limit`, `Penalty, every record`.

## Two map settings Power BI asks for by hand

Power BI only keeps a custom map file and a reference layer once you have chosen them in Desktop, so do this once and save:

- **Shape Map** (page 1). Select the map. In **Format visual, Visual, Map settings**, set the map type to **Custom map**, choose **Add map type**, and pick `..\data\geometry\community_districts.topojson`. Under **View map keys** the district codes (101, 102 ... 503) should match. If the Shape Map isn't in the Visualizations pane, turn it on in **Options, Preview features, Shape map visual**.
- **Azure Maps** (Map of the buildings page). Your organisation's admin must allow it (tenant setting "Use Azure Maps visual"); a personal account has it on by default. To see Midtown's footprints, go to **Format visual, Reference layer**, set the data source to **File** and choose `..\data\geometry\midtown_buildings.geojson`, with join property `key`. Use the **Grayscale (light)** or **Blank** map style for screenshots. Don't publish satellite basemap screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Districts[cd]`, Color saturation `[Penalty a year]`.
- Azure Maps: Location `Properties[property_name]`, Latitude `Properties[lat]`, Longitude `Properties[lng]`, Size `[Penalty a year]`.

Then save with **File, Save**.

## Save a .pbix too

After the refresh and the map settings, use **File, Save as** with the type **Power BI file (\*.pbix)** to save `powerbi\NYCLocalLaw97.pbix`. It includes the data, so it opens without a refresh.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Where the fines fall, 2030 to 2034 | Period 2030 to 2034 | still |
| 2 | The same, 2024 to 2029 | Period 2024 to 2029 | still |
| 3 | The period switching | Click between the two periods, about 1.5 s each | 5 s clip |
| 4 | Kinds of building | Period 2030 to 2034 | still |
| 5 | What is counted | Either period | still |
| 6 | Map of the buildings | Azure Maps with the Midtown reference layer, grayscale | still |
