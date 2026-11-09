# Lakeflow Designer and Genie Agents Tutorial

Hands-on Databricks workshops built around **Northgate Advisory LLP**, a fictional professional services firm. Attendees turn raw timesheets, expense claims, rate cards and Statement of Work PDFs into governed billing tables, then ask questions of that data with Genie and build an agent on top of it.

All people, clients, engagements and documents are synthetic.

## What's in the repo

```
1. Lakeflow Designer/
    1. Workshop Data Setup              Facilitator: loads the sample data (run once)
    2. Lakeflow Designer                No-code WIP flow, Genie Code, job scheduling, notebook comparison
    3. Extension - PDF Parsing          Parse SOW PDFs with ai_parse_document and ai_extract
    Maintainer - Regenerate Sample Data Only needed to change the scenario
    data/                               The shipped sample data (see below)
2. Genie Agents/
    1. Genie Agents                     Build, tune, benchmark and share a Genie Agent
    2. Document Intelligence            Genie over PDFs, Agent Bricks parsing and extraction
    3. Agents                           Build an agent in AI Playground and export it as an app
```

Run the workshops in that order. The Genie Agents notebooks use the tables and files from the Lakeflow Designer setup.

## Requirements

* A Unity Catalog-enabled workspace with serverless compute
* Permission to create a schema, tables and a volume in a catalog
* Lakeflow Designer, Genie and the AI functions (`ai_parse_document`, `ai_extract`) available in the workspace
* For *2. Document Intelligence*, the Genie file analysis preview enabled by a workspace admin

## Getting started (facilitator)

1. Clone or import this folder into the workspace.
2. Open **1. Lakeflow Designer / 1. Workshop Data Setup** and set the `catalog`, `schema` and `volume` parameters (defaults: your catalog, `consulting_billing`, `raw_files`).
3. Run the cells under the title. In a few seconds they create the schema and volume, copy the raw files and load the four master tables. Every file is checked against `data/MANIFEST.json`.
4. Set the same `catalog`, `schema` and `volume` parameters in each tutorial notebook.
5. During the scheduling exercise in *2. Lakeflow Designer*, run **Drop November Timesheet File** at the end of the setup notebook to simulate a new month arriving.

Re-running the setup resets the data, including removing the November file.

## The sample data

The data is generated once and shipped ready-made, so every workshop gets identical data and the answer keys in the notebooks always match.

| Folder in `data/` | Contents | Copied to |
| --- | --- | --- |
| `tables/` | `employees`, `clients`, `engagements`, `engagement_assignments` (JSON lines) | Delta tables in the schema |
| `timesheets/` | Timesheet exports, August-October 2026 (CSV) | `<volume>/timesheets/` |
| `expenses/` | Expense claims (JSON lines) | `<volume>/expenses/` |
| `reference/` | Grade rate card and monthly FX rates (CSV) | `<volume>/reference/` |
| `contracts/` | 16 Statements of Work and 1 change request (PDF) | `<volume>/contracts/` |
| `_facilitator/` | Answer keys for the AI extraction exercise | `<volume>/_facilitator/` |
| `designer_demo/` | Pre-built Designer prep for the PDF extension demo | `<volume>/_facilitator/` |
| `new_month/` | November timesheet file for the scheduling exercise | Copied only when you drop the November file |
| `MANIFEST.json` | A checksum for every file, plus table schemas and comments | Used by the setup notebook |

The timesheets include deliberate data quality issues for the exercises: weekend entries, days over 12 hours, time booked to engagements the employee isn't assigned to, and inconsistent formatting.

## Changing the scenario

Only maintainers need this. Edit and run **Maintainer - Regenerate Sample Data**. It uses a fixed random seed and writes everything back to `data/`, including a new `MANIFEST.json`. Then re-run the setup notebook and update the expected answers in the tutorial notebooks.

## Notes

* Attendees who share one schema should prefix the tables they create with their initials, for example `ab_engagement_wip_monthly`.
* The `_facilitator/` folder holds the answers. Don't attach the `raw_files` volume to a Genie Agent; *2. Document Intelligence* copies only the PDFs into a separate `contracts` volume for that.
