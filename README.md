# Demo project · Devon & Cornwall PBI User Group

Demos for *Working with AI to Build Dashboards People Understand*.
Source: `..\Data video games\vgchartz-2024.csv` (64,016 rows, 14 columns). All models point to this path.

## Presentation: three files

For the session itself you only use the `Presentation\` folder:

| File | Contents | Use |
|---|---|---|
| `Demo live.pbip` | copy of Demo 1 before: 1 flat table, Start and demo pages without results | **Demo in this one**, demo 1 to 8 in a row |
| `Demo final.pbip` | copy of Demo 8 after: everything done | Show the end result |
| `Demo tested.pbip` | Demo live after the test run of 27-10-2026: star schema, measures, genre pages and the arcade genre tooltip | Show what was already tested |

1. Open `Demo live.pbip` and click **Refresh** (the data isn't stored in the files).
2. Claude connects with `pbi connect`. After every Desktop restart Claude connects again (new port).
3. Commit after every successful demo, so you can go back to the last good point.
4. If a demo gets stuck: open `Demo N before.pbip` from the demo folder below and continue there.

Note: after Power Query changes (demo 1) and report changes (demos 3 to 8), Desktop has to reload the project.

**After a session:** reset `Demo live` to the clean start (tag `demo-live-start`):

```powershell
git restore --source=demo-live-start -- "Presentation/Demo live.pbip" "Presentation/Demo live.Report" "Presentation/Demo live.SemanticModel"
git clean -fd -- "Presentation/Demo live.Report" "Presentation/Demo live.SemanticModel"
```

A plain `git restore` from the last commit is not enough: the hourly auto-backup also commits test work.

## Safety net: one folder per demo with before and after

Every demo folder in `Safety net\` has two separate projects, each with **its own report and its own semantic model**:

- `Demo N before.pbip`: the starting point. Continue in this one when `Demo live` gets stuck.
- `Demo N after.pbip`: the result. Show this one when time runs out or the demo doesn't work.

| Folder | Demo | Before | After |
|---|---|---|---|
| `Demo 1 - star` | split the flat table into a star schema | 1 flat table, empty report | star schema |
| `Demo 2 - measures` | DAX measures | star schema | + measures |
| `Demo 3 - page` | build a page | + measures | + page |
| `Demo 4 - theme` | theme JSON | + page | + theme |
| `Demo 5 - tooltip` | tooltip sentence (Guy in a Cube) | + theme | + tooltip sentence |
| `Demo 6 - navigation` | navigation buttons | + tooltip sentence | + buttons |
| `Demo 7 - drillthrough` | drillthrough button | + buttons | + drillthrough |
| `Demo 8 - references` | references on the new card | + drillthrough | end result |

The "after" of demo N is the same as the "before" of demo N+1.

## Use

1. Open `Demo N before.pbip` and click **Refresh** (the data isn't stored in the files).
2. Demo live with Claude Code in that demo's folder.
3. If it doesn't work or there's no time: close it and open `Demo N after.pbip` (Refresh too).

**After a session:** resetting a before version to the original is done with git:

```powershell
git restore "Safety net/Demo 2 - measures"
git clean -fd "Safety net/Demo 2 - measures"
```

## Other folders

| Folder | What |
|---|---|
| `Material\prompts\` | Per demo: prompt · result · what I corrected. `screenshots\` before/after |
| `Material\` | Theme files `Theme.json` and `Theme 01.json` |

The old setup (`_oud\` with checkpoints and `tools\` with build scripts) was removed on 26-10-2026; it can be restored from git (commit `da8bdf3`). On 27-10-2026 the folders and files were renamed from Dutch to English (`Presentatie` → `Presentation`, `Vangnet` → `Safety net`, `Materiaal` → `Material`, `voor`/`na` → `before`/`after`, `Demo eind` → `Demo final`); older commits use the Dutch names.

## Note

- **Pause OneDrive** during the session.
- Demo 1 before is deliberately "the way Desktop does it": raw column names, numbers summarized as *Sum* (including `critic_score`), no measures, default theme.
