# 0003-d3-for-radial-layout

Use D3.js (v7, loaded from CDN) for the radial visualization and data-join logic.

D3's `d3.scaleRadial`, `d3.arc`, and enter/update/exit pattern are purpose-built for concentric ring layouts. We'd spend weeks reimplementing angle calculations, data binding, and transition choreography from scratch. D3 is the established standard for this kind of medical/clinical visualization (OHDSI Atlas, EPA EJScreen, etc.).

The CDN import means no bundler is needed — a single `<script>` tag in `index.html`. D3 is ~280KB minified, which is acceptable for a 10MB-cap app.

Rejected alternative: Vanilla Canvas/SVG math. More control, but the implementation cost is prohibitive for the radial + timeline synchronization we need.
