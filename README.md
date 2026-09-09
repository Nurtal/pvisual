# pvisual

A static, single-page OMOP patient data visualizer. It loads multi-file OMOP exports from a local directory and renders a radial visit-based visualization with a synchronized vertical timeline. No server, no database, no build step — one `index.html` opened via `file://`.

## Features

- **Zero-install**: a single `index.html` with all JS/CSS inlined (D3.js v7 from CDN). Open it by double-clicking; works in modern evergreen browsers (Chrome, Edge, Firefox, Safari).
- **Directory loading**: pick a folder of export files via the native directory picker.
- **Format auto-detection** based on file content:
  - **OMOP JSON** — one `.json` per table with a top-level OMOP table key (`"Person"`, `"Measurement"`, `"Visit_Occurrence"`).
  - **Synthea CSV** — one `.csv` per table with Synthea column names (`patients.csv`, `encounters.csv`, `observations.csv`).
- **CDM normalization** to an internal model based on OMOP CDM v5.4 (v5.3 and v5.4 OMOP JSON both supported). Synthea visits are synthesized from `encounters.csv` when no `visit_occurrence` table is present.
- **Radial visualization**: concentric rings represent visits (innermost = most recent); measurement nodes are evenly distributed along each ring, colored by concept. A center node shows patient identity, age, and death status.
- **Ring pagination**: at most 5 rings visible at a time, with `+`/`−` scroll rings to navigate a long visit history.
- **Timeline sidebar**: all measurements listed antéchronologically, with horizontal bars indicating visit membership.
- **Bidirectional sync**: clicking a ring highlights its measurements in the timeline; clicking a timeline entry highlights the corresponding node.
- **Node expansion**: clicking a measurement node opens an information panel (concept, value, unit, date) with a smooth CSS transition.

## Getting started

1. Clone the repo and open `index.html` directly in your browser:

   ```sh
   git clone git@github.com:Nurtal/pvisual.git
   open pvisual/index.html   # or double-click it
   ```

   > Directory selection uses the [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API). If `showDirectoryPicker` is unavailable, the app will tell you to use Chrome or Edge.

2. Click **Load patient data…** and select a directory of OMOP export files.
3. The radial visualization and synchronized timeline render for the first patient in the data.

## Sample data

`test-data/omop-json/person7/` contains a small OMOP JSON export (CDM v5.4) for a single patient — `Person`, `Measurement`, and `Visit_Occurrence` tables.

## Terminology

| Term | Meaning |
|------|---------|
| **Visit** | One healthcare encounter, represented as a ring. Innermost = most recent. |
| **Measurement** | A structured observation (lab result, vital sign) tied to a visit. |
| **Ring** | A concentric circle representing a single visit. |
| **Node** | A point on a ring representing one measurement. |
| **Center Node** | The innermost circle with patient identity, age, death status. |
| **Timeline** | The vertical sidebar listing all measurements antéchronologically. |
| **Concept** | A standardized clinical concept (`measurement_concept_id`); each maps to a distinct color. |

See `CONTEXT.md` for the full domain glossary.

## Documentation

- `CONTEXT.md` — domain glossary
- `docs/adr/` — architectural decisions (single HTML, SVG over Canvas, D3.js)
- `docs/specs/` — feature specifications
- `AGENTS.md` — project conventions and agent skills

## Supported formats

| Source | Detection | Details |
|--------|-----------|---------|
| OMOP JSON | top-level keys `Person` / `Measurement` / `Visit_Occurrence` | CDM v5.3 (date fields) and v5.4 (`visit_start_datetime`) |
| Synthea CSV | `patients.csv` / `encounters.csv` | visits synthesized from encounters; observations mapped to nearest visit |

File size is capped at 10 MB per file; a clear error is shown if a file exceeds the limit or if no recognizable data is found.

## Testing

This is a single-file app with no test framework. Verification is done via headless DOM integration tests (jsdom + D3) against the sample data in `test-data/`, plus manual browser testing for interactions (`showDirectoryPicker` requires a real browser context).
