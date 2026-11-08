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
3. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh**, on **Where the fines fall** with the Period slicer on **2024 to 2029** (as the report opens; it takes one period at a time):

| Card | Should read |
|---|---|
| Properties over the limit | 1,830 |
| Likely penalty a year | $58.57M ($58,573,596) |
| Share over the limit | 8.4% |
| Tonnes of CO2e over the limit | 218.6K (218,558) |

Pick **2030 to 2034** and they change to 13,546, $532.83M, 62.3% and 1.99M. These are the README's figures (1,830 of 21,753, about $58.6 million; 13,546, about $532.8 million).

The district chart starts with Manhattan 5 ($4.80M in 2024 to 2029).

## The model

- **Properties**: one row per kept property (`data/tables/properties.csv`), with both periods' emissions, limits and likely penalties, its pathway and data check. All the measures live here and count only `in_totals` rows, except `Penalty, every record` (the sensitivity page).
- **Districts**: the 59 community districts (`community_districts.csv`), joined to Properties by `cd`.
- **Period**: 2024 to 2029 and 2030 to 2034, for the slicer. The measures read the picked period's columns (2024 to 2029 when none is picked).
- Measures: `Properties`, `Over the limit`, `Share over`, `Penalty a year`, `Tonnes over`, `Emissions`, `Limit`, `Penalty, every record`.

## The two maps

- **Shape Map** (page 1). The custom map, `data/geometry/community_districts.topojson`, is saved inside the report (`NYCLocalLaw97.Report/StaticResources/RegisteredResources/community_districts.topojson`, a copy that keeps only the district code), so the 59 districts draw on open, shaded by the likely penalty for the period picked. The Shape Map has no base map of its own, so the file also carries the context: the counties round the city (New Jersey, Long Island, Westchester; US Census TIGERweb, public domain) in a pale ground tone, with the Hudson, the East River, the harbour and the ocean showing as the map's pale blue background. Parks and airports that belong to no district show as ground. Loading the plain file from `data/` by hand (below) gives the districts without that context. If the map shows the United States states instead (an older Desktop can ignore a saved map): select it, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\community_districts.topojson`. Under **View map keys** the district codes (101, 102 ... 503) should be matched. If the Shape Map is not in the Visualizations pane at all, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (Map of the buildings page). It opens on a light grey street map zoomed to the properties. One point per property (27,270 of them, close to the most Azure Maps plots): Location is the property id, latitude and longitude are averaged, the bubble is the likely penalty, the name and main use are in the tooltip. Your organisation's admin must allow Azure Maps (tenant setting "Use Azure Maps visual"); a personal account has it on by default. For Midtown's footprints add the reference layer by hand (Power BI can't take it from a file in the project): **Format visual**, **Reference layer**, **Data source: File** (or **URL**), and choose `..\data\geometry\midtown_buildings.geojson`. Use the **Grayscale (light)** or **Blank** map style for screenshots. Don't publish satellite basemap screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Districts[cd]`, Color saturation `[Penalty a year]`.
- Azure Maps: Location `Properties[property_id]`, Latitude `Properties[lat]` (Average), Longitude `Properties[lng]` (Average), Size `[Penalty a year]`, Tooltips `Properties[property_name]` (First).

## Save a .pbix too

After the refresh, use **File, Save as** with the type **Power BI file (\*.pbix)** to save `powerbi\NYCLocalLaw97.pbix`. It includes the data, so it opens without a refresh.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Where the fines fall, 2030 to 2034 | Period 2030 to 2034 | still |
| 2 | The same, 2024 to 2029 | Period 2024 to 2029 | still |
| 3 | The period switching | Pick each period in the Period dropdown, about 1.5 s each | 5 s clip |
| 4 | Kinds of building | Period 2030 to 2034 | still |
| 5 | What is counted | Either period | still |
| 6 | Map of the buildings | Azure Maps with the Midtown reference layer, grayscale | still |
