# Demo project video games

This is a live demo. Work only from the starting point and the question in the chat. The session is in English: write all answers, titles and explanations in English.

- Do **not** look in the "after" projects (`Demo N after.pbip`, `Demo N after.SemanticModel`, `Demo N after.Report`), nor in `Presentation/Demo final.*` and `Presentation/Demo tested.*` (the tested version of Demo live). They contain the solutions; don't read them, don't refer to them and don't copy anything from them, not via Bash, Grep or Glob either.
- Build solutions yourself from the model that is open in Power BI Desktop (usually `Presentation/Demo live.pbip`) and the source CSV.

## Way of working: pbi-cli + skills

This demo uses skills for the knowledge and pbi-cli for the connection to the model, as in the global CLAUDE.md.

- **Semantic model** (tables, columns, Power Query/M, relationships, measures, DAX): first load the matching pbi-cli skill (`power-bi-modeling`, `power-bi-partitions` for Power Query/M, `power-bi-dax` for DAX) and work through `pbi` after `pbi connect` with the model open in Desktop.
- **Report** (pages, visuals, theme, filters): load `anthropic-skills:powerbi-report-design` for the design and build with the pbi-cli skills `power-bi-report`, `power-bi-pages`, `power-bi-visuals`, `power-bi-themes` and `power-bi-filters`.
- Start every demo visibly: say which skill you load, connect with `pbi connect` to the model open in Desktop and show the current tables.
- The Power BI Modeling MCP (`powerbi-modeling-mcp`) is only the safety net: use it only when pbi-cli doesn't work, and say so.

## Speed (live demo)

- **At most 3 rounds.** (1) Load the skill, `pbi connect`, show the tables, `pbi database export-tmdl` (for the M source) and a short profiling query of about 10 checks, in one script. (2) Build everything plus the check query, in one script. (3) Explanation.
- **Choose yourself.** Make standard modelling choices without weighing them up front or asking, and report them afterwards in the explanation.
- **Put M in files in the scratchpad** and run all `pbi` calls of one step in one script.
- Keep the original flat table.

## Profiling

- **DAX first.** Check with `pbi --json partition list --table <table>` whether the table has data. If `state` is `NoData`, first refresh with `pbi table refresh <table> --type Full`. Then profile with DAX via `pbi dax execute --file` (rows, blank and distinct values, min/max, total against the sum of its parts), preferably in one query.
- **Safety net: PowerShell.** If DAX doesn't work, use `Import-Csv` on the source CSV. Read the CSV once and do all counts in the same script, because reading 64,000 rows is slow. Parse numbers and dates with `[Globalization.CultureInfo]::InvariantCulture` (the CSV uses a decimal point, Windows is set to nl-NL).
- Python doesn't work in the shell: `python` is the Microsoft Store alias.

## pbi pitfalls

- **Multi-line M** goes through a file: `pbi table create <name> --mode Import --m-expression - < file.m`. A shared query that isn't loaded: `pbi expression create <name> -e "$(cat file.m)"`; tables refer to it with `#"<name>"`.
- **Columns don't appear by themselves.** After `table create` with M the table only has `RowNumber`. Create every column with `pbi column create <column> -t <table> --data-type <type> --source-column <column>` and refresh afterwards.
- **Data types in lower case:** `string`, `int64`, `double`, `decimal`, `datetime`, `boolean` (so not `dateTime`).
- **Scripts in Bash** with `export MSYS_NO_PATHCONV=1`, then `< file.m` and `"$(cat file.m)"` work without trouble. Use relative paths in that script after `cd` to the scratchpad.
- **Relationships before the refresh.** If a relationship is only added after the refresh, DAX reports that the relationship contains no data until `pbi table refresh <table> --type Calculate`.
- **`pbi column` has no `update`:** the sort column (e.g. Month by Month Number) and summarizeBy can't be set through pbi.
- **`pbi partition` has no `get`:** read a table's M source via `pbi database export-tmdl <folder>`.
- **Saving.** Changes made through pbi only live in Desktop's memory; only after Ctrl+S in Desktop are they in the `.pbip`. Ask the user to save at the end.
- **Connection lost.** After a Desktop restart the port is different: run `pbi connect` again. If the connection is refused, check with `Get-Process PBIDesktop` whether Desktop is still running.

## Modelling rules (independent of MCP or CLI)

Profile the source columns before designing, and check above all the shape of the data, not just the values.

- **Unpivot wide columns.** If one kind of measure is spread over several columns (e.g. `na_sales`, `jp_sales`, `pal_sales`, `other_sales` = sales per region), unpivot them in Power Query (`Table.UnpivotOtherColumns`) into one value column plus an attribute column, and make that attribute its own dimension (e.g. `Region`).
- **No stored totals next to their parts.** A column that is the sum of other columns (e.g. `total_sales`) doesn't belong in the fact table; calculate the total with a measure. First check whether the total and the sum of the parts match, and report any differences.
- In the explanation, state explicitly which columns you unpivoted and why.
