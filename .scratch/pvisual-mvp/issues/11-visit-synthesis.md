# 11: Visit Synthesis (Synthea Fallback)

**What to build:** Load Synthea CSV directory without a `visit_occurrence` table but with `encounters.csv` → app synthesizes visits by grouping encounters by class, collapsing inpatient gaps. Rings appear correctly.

**Blocked by:** 10 (Synthea CSV Support)

**Status:** ready-for-agent

- [ ] Detects missing `visit_occurrence` table but present `encounters.csv`
- [ ] Groups encounters by `ENCOUNTERCLASS` into OMOP visit types: ambulatory/wellness/outpatient → 9202, emergency/urgentcare → 9203, inpatient → 9201
- [ ] Collapses consecutive inpatient encounters with ≤1 day gap between END and START into a single visit
- [ ] Synthesized visits added to `PatientData.visits` with correct visit_occurrence_id, dates, and concept_id
- [ ] Radial visualization renders synthesized visits as rings with correct visit-type colors
- [ ] Measurements from encounters are correctly associated with their synthesized visit
