# 04: Single Ring + Measurement Nodes

**What to build:** The innermost ring renders with its measurement nodes evenly spaced along the arc, colored by concept. Ring label shows visit date and type. Ring background color indicates visit category.

**Blocked by:** 03 (Center Node)

**Status:** ready-for-agent

- [ ] Innermost ring (most recent visit) rendered as a `<circle>` stroke around the center
- [ ] Ring label displays visit_start_date and visit_source_value (visit type)
- [ ] Measurement nodes rendered as `<circle>` elements on the ring, evenly distributed by angle
- [ ] Nodes colored by `measurement_concept_id` using `d3.schemeTableau10` palette
- [ ] Ring background stroke colored by visit type: inpatient=blue, outpatient=green, emergency=red
- [ ] All nodes are the same radius (uniform size)
- [ ] Color legend maps concept colors to concept names
