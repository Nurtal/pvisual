# Spec: pvisual — OMOP Patient Data Visualizer

## Problem Statement

Clinicians and researchers working with OMOP CDM patient data have no lightweight, portable way to visually explore a single patient's visit history and measurements. Existing tools (OHDSI Atlas, WebAPI) require a running database and server infrastructure. There is no zero-install, single-file viewer that loads OMOP exports from a local directory and renders an interactive radial timeline.

## Solution

A single `index.html` file that:
1. Prompts the user to select a directory containing OMOP export files (JSON or CSV).
2. Auto-detects the source format and CDM version (v5.3 or v5.4).
3. Normalizes the data to an internal model based on OMOP CDM v5.4.
4. Renders a radial visualization where concentric rings represent visits (innermost = most recent) and nodes on each ring represent measurements.
5. Displays a synchronized vertical timeline sidebar listing all measurements antéchronologically.
6. Supports bidirectional interaction: clicking a ring highlights the corresponding timeline entries, and clicking a timeline entry highlights the corresponding ring.
7. Expands clicked nodes into an information panel showing concept name, value, unit, and date, with a CSS transition animation.

## User Stories

### Data Loading & Detection

1. As a clinician, I want to select a directory of OMOP export files, so that I can load a patient's data without a database or server.
2. As a clinician, I want the app to auto-detect whether the files are OMOP JSON or Synthea CSV, so that I don't need to configure the format manually.
3. As a clinician, I want the app to auto-detect the CDM version (v5.3 or v5.4), so that the data is normalized correctly regardless of the source.
4. As a clinician, I want the app to handle OMOP JSON files where each table is a separate `.json` file with a top-level key (e.g., `"Person"`, `"Measurement"`), so that exports from tools like CU-DBMI/omop-fhir-data work directly.
5. As a clinician, I want the app to handle Synthea CSV files where each table is a separate `.csv` file with Synthea column names (e.g., `patients.csv`, `encounters.csv`, `observations.csv`), so that raw Synthea exports work directly.
6. As a clinician, I want the app to display an error message if the selected directory does not contain recognizable OMOP files, so that I know the input is invalid.
7. As a clinician, I want the app to display an error message if a file exceeds the 10MB size limit, so that I know the data is too large.
8. As a clinician, I want the app to gracefully handle missing optional tables (e.g., a directory with only `person.json` and `measurement.json` but no `visit_occurrence.json`), so that partial exports still load.
9. As a clinician, I want the app to synthesize visits from Synthea `encounters.csv` when no `visit_occurrence` table is present, so that Synthea raw exports still produce a valid ring layout.
10. As a developer, I want the data loading layer to be modular, so that new OMOP tables (e.g., `condition_occurrence`, `drug_exposure`) can be added later without rewriting the loader.

### Data Normalization

11. As a developer, I want the app to normalize all incoming data to an internal model based on OMOP CDM v5.4 field names, so that the rendering layer is format-agnostic.
12. As a developer, I want the internal data model to include at minimum: `person`, `visit_occurrence`, and `measurement` tables with their CDM v5.4 fields, so that the renderer has a stable contract.
13. As a developer, I want the normalization layer to map Synthea CSV column names to OMOP CDM v5.4 equivalents (e.g., `encounters.csv` → `visit_occurrence`, `observations.csv` → `measurement`), so that Synthea data is handled transparently.
14. As a clinician, I want the app to filter measurements by `person_id` after loading, so that only the selected patient's data is displayed (for multi-patient exports).

### Radial Visualization — Structure

15. As a clinician, I want the innermost ring to represent the most recent visit, so that the timeline reads naturally from center outward.
16. As a clinician, I want each ring to represent a single visit, so that I can visually distinguish visits.
17. As a clinician, I want measurement nodes to be evenly distributed along each ring's arc, so that the layout is balanced and readable.
18. As a clinician, I want each node to be colored by its `measurement_concept_id`, so that I can visually identify recurring measurement types across visits.
19. As a clinician, I want all nodes to be the same size, so that no single measurement is visually privileged over others.
20. As a clinician, I want the center node to display the patient's person_id, gender, and computed age, so that I can identify the patient at a glance.
21. As a clinician, I want the center node to display "Décédé(e)" and the patient's age at death if `death_date` is present, so that mortality status is immediately visible.
22. As a clinician, I want each ring to display a label with the visit date and visit type, so that I can identify which visit each ring represents.
23. As a clinician, I want each ring to have a background color that indicates the visit type (e.g., inpatient, outpatient, emergency), so that visit categories are visually distinguishable.

### Radial Visualization — Pagination

24. As a clinician, I want at most 5 rings visible at a time, so that the visualization remains readable for patients with many visits.
25. As a clinician, I want an outer "scroll ring" beyond the 5 visible rings, so that I can access older visits.
26. As a clinician, I want clicking the scroll ring to shift the visible window outward (showing older visits), so that I can navigate the full visit history.
27. As a clinician, I want the scroll ring to be visually distinct (e.g., dashed outline, `+` icon), so that I know it is interactive and not a real visit.

### Node Interaction

28. As a clinician, I want clicking a node to expand it into an information panel, so that I can see the full measurement details.
29. As a clinician, I want the expanded panel to show the concept name, value (value_as_number), unit, and measurement date, so that I have all clinically relevant information.
30. As a clinician, I want the node expansion to include a smooth CSS transition animation, so that the interaction feels polished and not jarring.
31. As a clinician, I want clicking outside the expanded panel (or on another node) to collapse it, so that I can dismiss details easily.

### Timeline Sidebar

32. As a clinician, I want a vertical timeline sidebar listing all measurements in antéchronological order (newest first), so that I can scan the patient's history linearly.
33. As a clinician, I want each timeline entry to show the measurement concept name, value, and date, so that I can identify measurements at a glance.
34. As a clinician, I want each timeline entry to have a horizontal bar on the side indicating which visit it belongs to, so that I can see visit membership without cross-referencing.
35. As a clinician, I want the horizontal bars to be colored to match the corresponding ring's visit-type background color, so that the visual link between timeline and ring is clear.

### Bidirectional Sync

36. As a clinician, I want clicking a ring in the radial view to highlight the corresponding measurements in the timeline sidebar, so that I can see all measurements for that visit in context.
37. As a clinician, I want clicking a measurement in the timeline sidebar to highlight the corresponding node on the ring, so that I can locate it in the radial view.
38. As a clinician, I want the highlighting to be visually distinct (e.g., ring pulses, timeline entry scrolls into view), so that the connection is unambiguous.

### Technical & Deployment

39. As a clinician, I want to open the app by double-clicking `index.html` (via `file://`), so that I don't need to install anything or run a server.
40. As a clinician, I want the app to work in modern evergreen browsers (Chrome, Firefox, Safari, Edge), so that I'm not restricted to a specific browser.
41. As a developer, I want the entire app inlined in a single `index.html` file (JS + CSS), so that it can be emailed, copied to a USB stick, or hosted as a static file.
42. As a developer, I want D3.js v7 loaded from CDN, so that no build step or `node_modules` is needed.
43. As a developer, I want the app to handle up to 10MB of source data, so that typical patient exports are supported.
44. As a developer, I want the visualization to remain responsive with up to 50 visits and up to 15 unique measurement concepts per visit, so that realistic patient histories are supported.
45. As a developer, I want the architecture to be modular (separate concerns for loading, normalization, rendering, interaction), so that the codebase is maintainable and testable.

## Implementation Decisions

### Architecture: Single HTML, Inlined Modules

The app is a single `index.html` with all JavaScript and CSS inlined. No build step, no bundler. JS is organized into logical sections within the file (data loading, normalization, radial rendering, timeline rendering, interaction). This follows ADR-0001.

### Rendering: SVG via D3.js

The radial visualization uses SVG rendered by D3.js v7 (CDN). Each ring is a `<g>` containing `<circle>` elements for nodes. CSS transitions handle the node-expand animation. This follows ADR-0002 and ADR-0003.

### Data Model Contract

The internal data model is the single seam between the data layer and the rendering layer:

```
PatientData {
  person: {
    person_id: string
    gender_source_value: string
    year_of_birth: number
    month_of_birth: number
    day_of_birth: number
    death_date: string | null
  }
  visits: Visit[]
}

Visit {
  visit_occurrence_id: string
  visit_concept_id: number
  visit_source_value: string
  visit_start_date: string
  visit_end_date: string
  measurements: Measurement[]
}

Measurement {
  measurement_id: string
  measurement_concept_id: number
  measurement_concept_name: string
  measurement_date: string
  value_as_number: string
  unit_source_value: string
  value_source_value: string
}
```

All downstream code (radial renderer, timeline, interaction) reads exclusively from this model. The loader and normalizer are the only code that touches raw files.

### Source Format Detection

Detection logic:
1. Scan the selected directory for files.
2. If any `.json` file contains a top-level key matching an OMOP table name (`Person`, `Measurement`, `Visit_Occurrence`), treat as OMOP JSON.
3. If any `.csv` file has Synthea column headers (`Id`, `PATIENT`, `START`, `STOP` in `encounters.csv`), treat as Synthea CSV.
4. If neither matches, display an error.

### CDM Version Detection

For OMOP JSON: check whether `visit_occurrence` fields include `visit_start_datetime` (v5.4) or only `visit_start_date` (v5.3). For Synthea CSV: always treat as equivalent to v5.4 after normalization.

### Visit Synthesis (Synthea Fallback)

When `visit_occurrence` is absent but `encounters.csv` is present, synthesize visits by grouping Synthea encounters by `ENCOUNTERCLASS`:
- `ambulatory`, `wellness`, `outpatient` → concept_id 9202 (Outpatient)
- `emergency`, `urgentcare` → concept_id 9203 (Emergency)
- `inpatient` → concept_id 9201 (Inpatient)

Collapse consecutive inpatient encounters with ≤1 day gap into a single visit.

### Color Palette

Use `d3.schemeTableau10` (10 distinguishable colors) cycled for additional concepts. Each unique `measurement_concept_id` maps to the next color in the palette. A legend maps colors to concept names.

### Ring Pagination

- Maximum 5 visible rings at a time.
- An outer dashed ring with a `+` icon indicates more visits exist.
- Clicking the scroll ring shifts the visible window outward by 5.
- Clicking an inner edge of the visible window shifts it inward.
- The center node is always visible and never counted as a ring.

### Node Expansion Animation

When a node is clicked:
1. The `<circle>` element transitions to a larger radius via CSS `transition: r 0.3s ease`.
2. An adjacent `<foreignObject>` containing the info panel fades in via CSS `opacity` transition.
3. Clicking elsewhere triggers the reverse transition and removes the panel.

### Timeline Layout

The timeline is a fixed-width sidebar (e.g., 320px) on the right side of the viewport. Each entry is a `<div>` with:
- Concept name (truncated with ellipsis if long)
- Value + unit (e.g., "82.7 U/L")
- Date
- A colored horizontal bar (4px) on the left edge, colored by visit type

Scroll position is managed programmatically when a ring is clicked (scroll the corresponding entry into view).

## Testing Decisions

### Testing Approach

Since this is a single-file app with no build step, testing is done via:
1. **Manual browser testing**: Load sample data, verify visual output, click interactions.
2. **Console-based validation**: A dev-mode flag that logs the normalized data model to the console for inspection.
3. **Sample data**: A `test-data/` directory containing the CU-DBMI `synthea-cohort-010` OMOP JSON dataset (10 patients) for development and testing.

### What to Test

- Data loading: directory selection, format detection, CDM version detection, error states.
- Normalization: Synthea CSV → OMOP v5.4 mapping, visit synthesis, measurement filtering by person_id.
- Rendering: correct number of rings, correct node count per ring, correct color mapping, center node displays correct patient info.
- Interaction: node click expands panel, click outside collapses, ring click highlights timeline, timeline click highlights ring.
- Pagination: scroll ring appears when >5 visits, clicking shifts the window, correct rings are visible after scroll.

### Prior Art

No existing tests in this repo. This is a greenfield project.

## Out of Scope

- Additional OMOP tables beyond `person`, `visit_occurrence`, `measurement` (modular architecture supports future addition, but not in v1).
- Server-side rendering, database connectivity, or API endpoints.
- Multi-patient comparison views (single-patient only).
- Data editing or export functionality.
- Mobile-responsive layout (desktop-first, modern browsers).
- Print-friendly output.
- Internationalization (UI text is in French/English as-is from the design session).
- Keyboard-only navigation (mouse/touch interaction first, accessibility second).
- TypeScript (plain JS per ADR-0001).

## Further Notes

- The sample dataset from CU-DBMI (`synthea-cohort-010`) should be included in `test-data/` for immediate development use. It contains 10 patients with referential integrity, OMOP JSON format.
- The app's UI language during the design session was mixed French/English. The implementation should use English for code and UI labels, with French only where the user explicitly requested it (e.g., "Décédé(e)" on the center node).
- Ring background colors for visit types: Inpatient = blue-toned, Outpatient = green-toned, Emergency = red-toned. Exact values TBD during implementation.
