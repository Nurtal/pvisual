# 0001-single-html-file

The entire application ships as one `index.html` with inlined JS and CSS. No build step, no bundler, no `node_modules`. Open via `file://` and it works.

This maximizes portability: clinicians can email the file, drop it on a USB stick, or open it on any machine with a modern browser. The trade-off is no module system, no TypeScript, no tree-shaking — we accept that for zero-friction deployment.

Rejected alternative: Vite-based static site. Better dev ergonomics, but requires a build step and produces multiple output files, breaking the single-file portability guarantee.
