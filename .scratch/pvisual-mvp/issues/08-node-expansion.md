# 08: Node Expansion

**What to build:** Click a measurement node → it expands into an info panel showing concept name, value, unit, and date. Smooth CSS transition animation. Click outside to collapse. Only one panel open at a time.

**Blocked by:** 07 (Bidirectional Sync)

**Status:** ready-for-agent

- [ ] Node click triggers expansion: circle grows in radius via CSS transition
- [ ] Info panel appears adjacent to the node (via `<foreignObject>` or overlay `<div>`)
- [ ] Info panel displays: measurement_concept_name, value_as_number, unit_source_value, measurement_date
- [ ] CSS transition: radius change + opacity fade (0.3s ease)
- [ ] Click outside the panel (or on another node) collapses the current panel with reverse transition
- [ ] Only one info panel open at a time; opening a new one closes the previous
