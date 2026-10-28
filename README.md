# Working with AI to Build Dashboards People Understand

Material from a live demo session for the Devon & Cornwall Power BI User Group, showing how to build a Power BI report end-to-end with an AI coding agent (Claude Code) instead of building it by hand: turning one flat CSV export into a star-schema model with measures, a themed report and interactive navigation.

The dataset is video game sales (`vgchartz-2024.csv`, ~64,000 rows) — not included in this repo, see [Data](#data) below.

## What's in this repo

| File | What it is |
|---|---|
| `Demo live.pbip` | The starting point for the session: one flat table, no model built yet |
| `Demo tested.pbip` | A full dry run of the session, already built and tested, for reference |
| `Demo final.pbip` | The finished result: star-schema model, measures, formatted and interactive report pages |
| `CLAUDE.md` | The instructions given to the AI agent for this project — how it should work with the semantic model and the report |
| `Screenshot page by Claude.png` | The same report page, built with Claude Code |
| `Screenshot page by Git Hub Copilot*.png` | The same report page, built with GitHub Copilot, for comparison |

## Using this yourself

1. Get the source data (the VGChartz 2024 video game sales export, `vgchartz-2024.csv`) and update the Power Query source step to point at your own copy — the file itself isn't part of this repo.
2. Open any `.pbip` file in Power BI Desktop and click **Refresh**; the data isn't stored in the project files, only the model and report definitions are.
3. Open `Demo live.pbip` to start from the raw flat table, or `Demo final.pbip` to see the end result directly.
4. `CLAUDE.md` shows the actual working instructions used to drive the build with Claude Code.

## Data

The dataset used is a video game sales export (`vgchartz-2024.csv`, 64,016 rows, 14 columns: title, platform, genre, publisher, regional and global sales, critic score, release date). It is not included in this repo; source your own copy and point the model's Power Query source at it before refreshing.

## Notes

- This is session material, not a maintained project — some paths assume the pbi-cli / MCP setup described in `CLAUDE.md`.
- `Demo live` is deliberately left "the way Power BI Desktop builds it by default": raw column names, numbers auto-summarized, no measures, default theme — the starting point the rest of the session improves on.
