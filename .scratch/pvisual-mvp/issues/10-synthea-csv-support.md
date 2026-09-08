# 10: Synthea CSV Support

**What to build:** Load a directory of Synthea CSV files → app detects CSV format, normalizes Synthea column names to OMOP CDM v5.4, produces the same `PatientData` model. Visualization works identically to JSON-loaded data.

**Blocked by:** 09 (Ring Pagination)

**Status:** ready-for-agent

- [ ] Format detection: scans directory for `.csv` files with Synthea headers (`Id`, `PATIENT`, `START`, `STOP` in encounters; `Id`, `PATIENT`, `DATE`, `CODE`, `VALUE` in observations)
- [ ] Column name mapping: `patients.csv` → person fields, `encounters.csv` → visit_occurrence fields, `observations.csv` → measurement fields
- [ ] Normalizes to identical `PatientData` model as the JSON path
- [ ] CDM version treated as v5.4 after normalization
- [ ] Visualization renders identically to JSON-loaded data (same rings, nodes, timeline)
- [ ] Error handling for corrupt CSV or missing required columns
