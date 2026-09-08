# pvisual

A static, single-page OMOP patient data visualizer. Loads multi-file OMOP exports from a directory, renders a radial visit-based visualization with a synchronized vertical timeline. No server, no database — one `index.html` opened via `file://`.

## Project structure

- `index.html` — single-file app (JS + CSS inlined, D3.js v7 from CDN)
- `test-data/` — sample OMOP JSON datasets for development
- `CONTEXT.md` — domain glossary (visit, measurement, ring, node, center node, timeline, concept, etc.)
- `docs/adr/` — architectural decisions (single HTML, SVG over Canvas, D3.js)
- `docs/specs/` — feature specifications
- `docs/agents/` — agent skill configuration

## Agent skills

### Issue tracker

GitHub Issues. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-role vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context. One `CONTEXT.md` at root + `docs/adr/`. See `docs/agents/domain.md`.

## Conventions

- Single `index.html`, no build step, no bundler (ADR-0001)
- SVG via D3.js v7 (ADR-0002, ADR-0003)
- Plain JavaScript, no TypeScript (ADR-0001)
- Modern evergreen browsers only
- Use domain glossary terms from `CONTEXT.md` in all output
