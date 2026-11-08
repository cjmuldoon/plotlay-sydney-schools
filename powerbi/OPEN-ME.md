# Opening the Power BI report

`SydneySchoolGrounds.pbip` is a Power BI Project: the report (`SydneySchoolGrounds.Report`, PBIR format) and the semantic model (`SydneySchoolGrounds.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). Projects and the PBIR report format are standard in current versions. If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine, wherever you cloned or unzipped it. On a Mac running Windows in Parallels the Mac home folder is shared, usually as `\\Mac\Home`.

## Open it

1. In Windows, open File Explorer and go to the repo's `powerbi` folder (wherever you cloned or unzipped `plotlay-sydney-schools`).
2. Double-click `SydneySchoolGrounds.pbip`. If Windows asks which app, choose Power BI Desktop. Or in Desktop: **File, Open report, Browse reports**, set the file type to *Power BI project files (\*.pbip)* and pick it.
3. The report opens with empty visuals the first time, because nothing has been loaded yet. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-sydney-schools/main/data/`). If Desktop asks how to connect to that address, choose **Anonymous** and **Connect**.
   - To read the files from the folder on disk instead (no internet, or before the repo is public): **Home, Transform data, Edit parameters**, set `DataFolder` to the full path of the repo's `data` folder **ending with a backslash**, for example `C:\Users\you\plotlay-sydney-schools\data\`, then **OK** and **Apply changes**. Power BI can't use a path relative to the project file, so this one parameter is the only place a folder is named.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** for this file (all the sources are public open data) or set both sources to Public.
4. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh**, with the Year slicer on **2025** (as the report opens; it allows one year at a time):

| Overview card | Should read |
|---|---|
| Median, all compared schools, m² per student | 30.7 |
| Median high school, m² per student | 34.7 |
| Median primary school, m² per student | 30.3 |
| Students in the study area | 41,439 (Power BI may show 41.4K) |

The bar chart lists the 15 compared schools with the least ground per student, smallest first: Parramatta HS 7.0, Balmain PS 10.9, Woollahra PS 10.9, Ultimo PS 11.1, Neutral Bay PS 12.8, and so on.

Set the slicer to **2015** and the cards change to 26.0, 31.8 and 25.1 m² per student and 37,248 students.

On the **Attendance** page the card reads **68** (of 72 schools with both years), and the line runs from 93.7% in 2019 to 90.3% in 2024. Drill through from the bar chart to **Parramatta HS**: 7,950 m² of grounds, 1,128 students in 2025, 7.0 m² per student.

If the medians read about 1.5 or the students about 789K, the report is an old copy: close it and open this version.

## The two maps

- **Shape Map** (Overview page). The custom map, `data/geometry/schools.topojson`, is saved inside the report (`SydneySchoolGrounds.Report/StaticResources/RegisteredResources/schools.topojson`), so the school shapes show on open, coloured by m² per student. If the map shows the United States instead (an older Desktop can ignore a saved map): select the map, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\schools.topojson`. Under **View map keys** the school codes (8913, 8135...) should be matched. If the Shape Map is not in the Visualizations pane at all, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (Map detail page). The points are plotted from each school's latitude and longitude (averaged, one point per school), with the school code in Location and the name in the tooltip. Azure Maps must be allowed by your organisation's admin (Power BI admin portal, tenant setting "Use Azure Maps visual"); on a personal account it is on by default. For the filled school shapes add the reference layer by hand (Power BI can't take it from a file in the project): select the map, **Format visual**, **Visual**, **Reference layer**, **Data source: File** (or **URL**), and choose `..\data\geometry\schools.geojson`. Its `key` property is the school code, the same value as the map's Location. Set the map style to **Grayscale (light)** or **Blank** for screenshots: do not publish satellite basemap screenshots.

If either map visual comes up empty (Power BI sometimes drops the fields of a map visual it can't draw yet), drag the fields back: Shape Map Location `Schools[map_key]`, Color saturation `[m2 per student]`; Azure Maps Location `Schools[map_key]`, Latitude `Schools[latitude]` (Average), Longitude `Schools[longitude]` (Average), Size `[Students]`, Tooltips `Schools[school_name]` (First).

Then **File, Save**. Desktop writes your choices back into the project folder.

## Save a .pbix too

A `.pbix` is a single file anyone can open, but it can only be written by Power BI Desktop. After the refresh (and the Azure Maps reference layer, if you want it): **File, Save as**, set the file type to **Power BI file (\*.pbix)**, and save it as `powerbi\SydneySchoolGrounds.pbix` next to the project. It includes the data, so it opens without a refresh. Commit it if you want people to download one file (it is a few MB).

## Publish (only if you choose to)

1. **Home, Publish**, sign in, pick a workspace.
2. In the Power BI service, open the semantic model's **Settings, Data source credentials**: the GitHub address uses **Anonymous**. Scheduled refresh then works with no gateway, because the data is read from the public GitHub copy.
3. **Publish to web** makes a public embed link. It is turned off in many organisations, and anyone with the link can see the report. Only use it if you intend the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme on. In Parallels, use the Windows Snipping Tool (Win+Shift+S) for stills and Win+Alt+R (Xbox Game Bar) or Clipchamp for a clip. Save them to `powerbi/screenshots/` and the website team swaps them into the page.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Overview, 2025 | Year slicer on 2025 (as it opens), nothing selected | still |
| 2 | Overview, 2020 | Year slicer on 2020: the new schools pale, newly opened | still |
| 3 | The year stepping | Pick 2015, 2018, 2020, 2022, 2025 in the Year dropdown, about 1 s each | 6 to 8 s clip |
| 4 | Drill-through | Right-click Parramatta High in the bar chart, **Drill through, School** | still, then a 3 s clip of the click |
| 5 | Map detail | Azure Maps with the reference layer, grayscale style | still |
| 6 | Over time | The two line charts | still |
| 7 | Model view | The four tables and their relationships | still |
| 8 | Power Query | **Transform data**: the `DataFolder` parameter and a table's steps | still |

Keep the footer with the attribution visible in every shot. Don't show satellite basemaps from Azure Maps.

## What's in the folder

- `SydneySchoolGrounds.pbip`: open this.
- `SydneySchoolGrounds.SemanticModel/definition/`: `tables/*.tmdl` (columns, measures, Power Query), `relationships.tmdl`, `expressions.tmdl` (the `DataFolder` parameter and a small `GetFile` function).
- `SydneySchoolGrounds.Report/definition/`: one folder per page and visual (PBIR), `report.json` (the Plotlay theme registered), `StaticResources/RegisteredResources/Plotlay_drafting_film.json` (the theme).
- Pages: Overview, Map detail, Over time, Attendance, and School (drill-through target, by school code).
