# HTML Visual Template — service-planning-harness

> In the final output stage, render the generated planning deliverables (.md) as a **self-contained HTML dashboard that is easy to read and centered on diagram visualization**.
> Goal: see **the structure of every deliverable at a glance**. Instead of copy-pasting the body text, visualize the structure and essentials with mind maps, ecosystem maps, diagrams, flowcharts, sequence diagrams, state-transition diagrams, ERDs, Gantt charts, architecture diagrams, charts, infographics, and webtoon panels (invent a topic-specific visualization when a standard diagram doesn't fit), and keep the details in collapsible sections. **Everything is inline SVG/CSS + generated images when possible — no external renderer (Mermaid etc.).** Because it's infographic-first, it is always included in the STEP 1-b default ("Both").

## When / What

- **Modes 1–4 (single document)**: one `<service>-plan.html` file — section the document for that mode + visualizations matching the mode (only the applicable ones from the catalog below).
- **Mode 5 (full package)**: one `docs/index.html` file — a dashboard that synthesizes the 5 stages / 16 document types into the 13-type visualization catalog. (Orchestrator: render right after the STEP 7 wireframe checkpoint, before STEP 8 delivery.)
- Render input = the already-PASSED final deliverables (.md). The HTML **must not generate new content** — only visualize the structure, numbers, and relationships of the existing deliverables (zero placeholders; numbers cited only from the deliverables and `00-business-model.md`).

## Technical policy (self-contained, one file, zero external dependencies)

- A single `.html`. CSS/JS/SVG **all inline**. **Zero external dependencies** — every visualization (diagrams, flowcharts, sequence diagrams, ERDs, Gantt charts, ecosystem maps, charts, infographics, webtoons) is built with **hand-authored inline SVG/CSS**. **No external renderers/CDNs/plugins/web fonts such as Mermaid** (self-contained, offline, design control, Korean-safe).
  - Why not Mermaid: external CDN dependency (breaks offline / on intranets) + auto-layout means weak control over design and Korean labels and it turns sloppy. Hand-written SVG meets the infographic-first quality bar.
- **Generated images (when possible)**: for persona webtoons, ecosystem illustrations, concepts, etc., if you have image-generation capability, create them in `assets/` and embed via `<img>` (local relative path). Without that capability, fall back to inline SVG/CSS.
- A one-line comment at the top: "Fully self-contained (inline SVG/CSS) — works offline, no external dependencies."
- A restrained palette with one accent (e.g., indigo #4f46e5 + secondary teal). Dark text, light background. System fonts. No slop — at the level of a deliberately designed internal dashboard (whitespace, hierarchy, scannability).

## Visualization catalog (deliverable → visualization → technique)

> Technique = **all inline SVG/CSS** (no external renderers such as Mermaid). Webtoons/illustrations use generated images (`assets/`) when possible, otherwise CSS/SVG.

| # | Visualization | Source document | Technique (inline SVG/CSS) |
|---|--------|-----------|------------------------|
| 1 | **Overall structure mind map** (service → stage → document/feature) | All / PRD | Inline SVG radial node-edge — placed in the hero, "whole structure at a glance" |
| 2 | **Core loop/mechanism diagram** (e.g., two-sided value cycle) | Persona / plan | Inline SVG circular loop (arrows, labels) |
| 3 | **Ecosystem map** (participants, value network) | 02-market-competition | Inline SVG nodes (participants) + directed edges (value/money/data flow, labeled), own node highlighted |
| 4 | **Revenue/money flow infographic** | 00-business-model | Inline SVG (split, pool distribution, amounts, Phase tags, Sankey-style) |
| 5 | **Unit economics chart** (breakdown, BEP) | 00-business-model | Inline SVG bar/donut |
| 6 | **Competition matrix + positioning scatter** | 02-market-competition | CSS heatmap (✓/△/✗ color chips, own row highlighted) + inline SVG 2-axis scatter |
| 7 | **Persona webtoon panels** (situation → trigger → pain → resolution) | 03-personas | Generated image cuts when possible, otherwise CSS comic cuts + speech/thought bubbles |
| 8 | **Persona journey map** (actions/emotions per stage) | 03-personas | Inline SVG horizontal timeline + emotion curve |
| 9 | **User flow flowchart** (per-role paths, screen/feature tags on nodes) | 13-user-flows | Inline SVG node-edge (Phase 2 dashed) |
| 10 | **API/consent/settlement sequence diagram** (participant lanes + message arrows) | 32/33/00 | Inline SVG lifelines + numbered arrows |
| 11 | **State-transition diagram** (screen states: empty/loading/error/normal) | 21-screen-spec | Inline SVG state nodes + transition arrows |
| 12 | **IA screen tree** (screen-ID hierarchy, color per role) | 12-ia | Inline SVG tree (TD) |
| 13 | **System architecture** (client → API → data → external layers) | 30/31/32 | Inline SVG layer boxes + connectors |
| 14 | **ERD diagram** (core entities, relationships) | 31-erd | Inline SVG entity boxes + 1:N relationship lines (crow's-foot) (full list collapsible) |
| 15 | **Success-metric gauge/bar chart** (target values) | 01/06 metrics | Inline SVG/CSS, color per axis |
| 16 | **MVP vs Phase 2 scope infographic** | 01/10 | CSS 2-column VS (included/excluded color-coded) |
| 17 | **Backlog Gantt** (sprint placement, critical path) | 40-backlog | Inline SVG horizontal bar timeline + critical path highlighted |
| (topic-specific invented) | When a standard diagram doesn't fit the concept, invent a visualization just for that topic (funnel, heatmap, ecosystem quadrant, etc.) | the relevant document | Inline SVG/CSS — creatively, fit to the domain |

> Modes 1–4 use only the applicable ones above (e.g., Mode 1 Lean → 1·2·3·6·9·15·16). Mode 5 uses all + topic-specific inventions. Every visualization is inline SVG/CSS (zero external dependencies).

## Layout

- Fixed left **sidebar TOC** (stage/document groups, jump) + main scroll (content max-width ~960px, line-height 1.7). Hamburger toggle on narrow screens (<900px).
- Sticky top bar: service name + one-line summary + meta (date written, deliverable count) + `[🖨 Print/PDF]` (window.print).
- Hero: one-line value + stat cards (mode, deliverable count, MVP duration, one-line revenue model) + **overall structure mind map (#1)**.
- Per-stage sections: each section has its visualization + below it **collapsible (`<details>`) detailed essentials** (tables, lists, badges).
- **Badges**: priority P0 (red)·P1 (orange)·P2 (gray), Phase MVP (green)·Phase 2 (gray). ID tokens (`F1`·`SC-02`·`P3`·`T1`·`E12`) auto-styled as monospace chips.
- Interaction: sidebar active highlight (IntersectionObserver), smooth scroll, collapsible groups.

## Self-contained skeleton (fill the slots)

```html
<!doctype html><html lang="ko"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{TITLE}} — Planning dashboard</title>
<style>
/* variables: --accent, --accent2, --bg, --fg, --muted, --line; badge/chip/card/table/webtoon/print CSS */
/* layout: .layout{display:grid;grid-template-columns:260px 1fr} .sidebar{position:sticky;top:0;height:100vh;overflow:auto} */
/* tables: wrapper overflow-x:auto, zebra, sticky thead; pre: preserve monospace; .callout highlight box */
/* @media print{ .sidebar,.topbar,button{display:none} details{open} *{break-inside:avoid} main{width:100%} } */
/* @media (max-width:900px){ .layout{grid-template-columns:1fr} .sidebar{position:fixed;transform:translateX(-100%)} .sidebar.open{transform:none} } */
</style></head>
<body>
<header class="topbar">{{TITLE}} · {{ONELINE}} · {{META}} <button onclick="window.print()">🖨 Print/PDF</button></header>
<div class="layout">
  <nav class="sidebar" aria-label="TOC">{{TOC}}</nav>
  <main>
    <section id="hero">{{ONELINE}} {{STAT_CARDS}} <figure class="viz">{{MINDMAP_SVG}}</figure></section>
    {{MONEY_CALLOUT}}
    {{SECTIONS}}  <!-- per stage: visualization (inline <svg>/CSS, or <img src="assets/..">) + <details> detail -->
  </main>
</div>
<script>
  // inline SVG/CSS only — no external renderer/CDN (Mermaid not used).
  // IntersectionObserver active TOC highlight; hamburger toggle; beforeprint → details[open]=true
</script>
</body></html>
```
> Note: `lang="ko"` is a default; set it to match the chosen output language (deliverable language is user-selectable, default Korean — the HTML just renders whatever language the .md is in).

## Rules (check during audit)

- **Visualization is the lead**: fill the relevant catalog items **with real data** (no empty diagrams or placeholders).
- **Content-faithful, no generation**: visualize only the deliverables' structure, numbers, and relationships. Money rules only cite `00-business-model.md` (no redefining). Don't invent numbers not in the deliverables.
- **Self-contained**: zero external dependencies — all visualizations inline SVG/CSS, **no Mermaid/CDN/web fonts/plugins**. Images are local in `assets/`. Opens on double-click, works fully offline.
- **Print**: hide sidebar/buttons, expand collapsibles, page-break-inside avoid, inline SVG prints as-is.
- **Accessibility**: semantic landmarks (nav/main/header), aria toggles, sufficient contrast, focus styles.
- **Korean**: tables/chips word-break safe, no overflow clipping.
- A deliberately designed dashboard — no generic Bootstrap dump.

## Reference instance

`character-market/docs/index.html` (Mode 5, one file ~86KB) — for layout/composition reference. **However, if this instance used a Mermaid CDN, that conflicts with the current policy** — new deliverables must be entirely inline SVG/CSS (zero external renderers). Reference only its layout/information structure, and replace the diagram technique with inline SVG.
