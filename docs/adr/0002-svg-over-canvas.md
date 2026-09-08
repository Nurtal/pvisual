# 0002-svg-over-canvas

Use SVG (via D3.js) for the radial visualization rather than HTML Canvas.

SVG gives us DOM-based elements with native event handling (click, hover), CSS transitions for the node-expand animation, and easy accessibility (each node is a `<circle>` or `<g>` with aria attributes). The patient data we're visualizing is bounded (≤10MB, ≤50 visits, ≤15 unique concepts per visit), so SVG's per-element overhead is irrelevant.

Canvas would be warranted only if we needed thousands of animated nodes or WebGL-level rendering. We don't.

Rejected alternative: HTML Canvas with custom hit-testing. Faster for massive datasets, but we'd lose CSS transitions, native events, and accessibility for no practical gain.
