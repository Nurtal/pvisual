# 09: Ring Pagination

**What to build:** Patients with more than 5 visits show 5 rings at a time plus an outer dashed scroll ring. Clicking the scroll ring shifts the visible window outward. Clicking the inner edge shifts it back. Center node always visible.

**Blocked by:** 08 (Node Expansion)

**Status:** ready-for-agent

- [ ] Maximum 5 rings visible at any time
- [ ] Outer scroll ring rendered with dashed stroke and a `+` icon when total visits > 5
- [ ] Click scroll ring → visible window shifts outward by 5 (older visits appear, newest may scroll off)
- [ ] Click inner edge of visible window → window shifts inward (newer visits reappear)
- [ ] Scroll ring disappears when all visits are visible (≤5 total)
- [ ] Center node is always visible and never affected by pagination state
