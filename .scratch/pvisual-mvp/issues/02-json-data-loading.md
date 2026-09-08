# 02: JSON Data Loading → PatientData

**What to build:** The complete data loading pipeline: user selects a directory, app auto-detects OMOP JSON format, parses all files, normalizes to the `PatientData` internal model, and logs it to the console. Error states for every failure mode.

**Blocked by:** 01 (Scaffold + Sample Data)

**Status:** ready-for-agent

- [ ] Directory picker via `<input type="file" webkitdirectory>` (or equivalent)
- [ ] Format detection: scans directory for `.json` files containing OMOP top-level keys (`Person`, `Measurement`, `Visit_Occurrence`)
- [ ] CDM version detection: checks whether `visit_occurrence` records include `visit_start_datetime` (v5.4) or only `visit_start_date` (v5.3)
- [ ] Parses JSON files and normalizes to `PatientData` model (person, visits array with measurements)
- [ ] Filters data by `person_id` — for multi-patient exports, prompts user or uses first patient
- [ ] 10MB file size check: displays error if any single file exceeds limit
- [ ] Graceful handling of missing optional tables (e.g., no `visit_occurrence` → empty visits array)
- [ ] Error message displayed for unrecognized file formats or corrupt JSON
- [ ] Full `PatientData` object logged to console for inspection
