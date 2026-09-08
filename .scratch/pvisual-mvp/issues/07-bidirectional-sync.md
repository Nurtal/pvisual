# 07: Bidirectional Sync

**What to build:** Clicking a ring highlights that visit's measurements in the timeline. Clicking a timeline entry highlights the corresponding node on the ring. Visual feedback is clear and unambiguous.

**Blocked by:** 06 (Timeline Sidebar)

**Status:** ready-for-agent

- [ ] Click a ring → all timeline entries belonging to that visit get a highlighted style (e.g., background color change, border)
- [ ] Click a timeline entry → the corresponding measurement node on the ring gets a highlighted style (e.g., stroke, scale)
- [ ] Timeline auto-scrolls to bring the first highlighted entry into view
- [ ] Clicking empty space (SVG background or timeline gap) clears all highlights
- [ ] Only one visit highlighted at a time in each direction
