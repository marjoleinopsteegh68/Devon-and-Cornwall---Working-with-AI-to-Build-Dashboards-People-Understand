# Working with AI to Build Dashboards People Understand

Material from a live demo session for the Devon & Cornwall Power BI User Group, showing how to build a Power BI report end-to-end with an AI coding agent (Claude Code) instead of building it by hand: turning one flat CSV export into a star-schema model with measures, a themed report and interactive navigation.

The dataset is video game sales (`vgchartz-2024.csv`, ~64,000 rows), included in this repo — see [Data](#data) below.

## What's in this repo

| File | What it is |
|---|---|
| `Demo live.pbip` | The starting point for the session: one flat table, no model built yet |
| `Demo tested.pbip` | A full dry run of the session, already built and tested, for reference |
| `Demo final.pbip` | The finished result: star-schema model, measures, formatted and interactive report pages |
| `CLAUDE.md` | The instructions given to the AI agent for this project — how it should work with the semantic model and the report |
| `Screenshot page by Claude.png` | The same report page, built with Claude Code |
| `Screenshot page by Git Hub Copilot*.png` | The same report page, built with GitHub Copilot, for comparison |
| `vgchartz-2024.csv` | The source dataset |
| `vg_data_dictionary.csv` | Column descriptions for the source dataset |

## Using this yourself

1. Open any `.pbip` file in Power BI Desktop. Each one opens with its full dataset already loaded from the cached semantic model — no setup needed.
2. Click **Refresh** if you want Power BI to re-pull the data through its original Power Query source step; this will fail unless you first point that step at your own local copy of `vgchartz-2024.csv` (from this repo), since the original file path is local to the original author's machine.
3. Open `Demo live.pbip` to start from the raw flat table, or `Demo final.pbip` to see the end result directly.
4. `CLAUDE.md` shows the actual working instructions used to drive the build with Claude Code.
5. From here you can do anything you'd do with your own Power BI file: explore the data, change the model, rebuild the report, export the data, or use it as a starting point for your own dashboard.

## Data

The dataset is a video game sales export (`vgchartz-2024.csv`, 64,016 rows, 14 columns: title, platform, genre, publisher, regional and global sales, critic score, release date), with `vg_data_dictionary.csv` describing each column. It's also cached inside each `.SemanticModel/.pbi/cache.abf`, so it travels with the `.pbip` files and loads automatically when you open them.

## Notes

- This is session material, not a maintained project — some paths assume the pbi-cli / MCP setup described in `CLAUDE.md`.
- `Demo live` is deliberately left "the way Power BI Desktop builds it by default": raw column names, numbers auto-summarized, no measures, default theme — the starting point the rest of the session improves on.
