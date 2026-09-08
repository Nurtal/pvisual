# 03: Center Node

**What to build:** The center node renders at the SVG origin, displaying patient identity information from the `PatientData` model. Load data → see a circle with person_id, gender, age, and death status.

**Blocked by:** 02 (JSON Data Loading → PatientData)

**Status:** ready-for-agent

- [ ] Center `<circle>` rendered at SVG center via D3 data bind
- [ ] Displays person_id text
- [ ] Displays gender_source_value (e.g., "F", "M")
- [ ] Computes age from year_of_birth, month_of_birth, day_of_birth (relative to current date or death_date)
- [ ] If death_date is present: displays computed age at death and "Décédé(e)" label
- [ ] If death_date is absent: displays current age
- [ ] Clean typography, centered text within the circle
