# 05: All Rings (Full Radial)

**What to build:** All visits render as concentric rings, innermost = most recent, outermost = oldest. The full patient visit history is visible at a glance in the radial layout.

**Blocked by:** 04 (Single Ring + Measurement Nodes)

**Status:** ready-for-agent

- [ ] All visits from `PatientData.visits` rendered as concentric rings
- [ ] Visits sorted by visit_start_date descending (most recent = innermost)
- [ ] Ring radius increases monotonically with age (older visits further from center)
- [ ] Each ring has its own set of measurement nodes
- [ ] Ring labels and visit-type background colors consistent across all rings
- [ ] Layout remains balanced with 1–50 visits (no overlap, no invisible rings)
