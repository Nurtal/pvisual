# 01: Scaffold + Sample Data

**What to build:** The foundational single-file app shell with D3 loaded, CSS layout in place, and sample OMOP JSON data ready for development. Open `index.html` via `file://` → see a layout skeleton with a radial SVG area and a timeline sidebar placeholder.

**Blocked by:** None (can start immediately).

**Status:** ready-for-agent

- [ ] `index.html` created with D3.js v7 loaded from CDN (`<script>` tag)
- [ ] CSS layout: radial SVG area (left/center) + timeline sidebar (right, 320px width)
- [ ] `test-data/` directory populated with CU-DBMI `synthea-cohort-010` OMOP JSON files: at minimum `Person_0000000000.json`, `Visit_Occurrence_0000000000.json`, `Measurement_0000000000.json`
- [ ] File opens via `file://` protocol without console errors
- [ ] D3 is accessible in global scope (verify with `console.log(d3.version)`)
