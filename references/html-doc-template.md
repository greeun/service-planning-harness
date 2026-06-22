# HTML Doc Template — service-planning-harness

> Additionally render the deliverable `.md` files as **self-contained HTML that is easy to read, share, and review**. The **original `.md` is always preserved** (HTML is an additional output).
> There are two kinds of HTML (don't confuse them):
> - **This template (html-doc-template)** = per-document **readable HTML** + hub `index.html`. For readability, sharing, and review. Render the body to read well, but **infographic-first**: preserve the md's ASCII diagrams in monospace, promote the data in the md (tables, trees, flows, matrices) into **data-driven inline SVG/CSS infographics**, and display generated image assets (`assets/`). **No external renderer dependency (Mermaid discouraged).**
> - `html-visual-template.md` = **diagram-rich dashboard** (mind maps, charts, infographics). For seeing "the structure at a glance." When "Both" is selected, also produce it as `overview.html`.

## When (selected by the user in STEP 1)

In activation STEP 1, ask for the **HTML output form** along with the mode:
- **Per-document HTML + hub index.html** [recommended] — this template. md preserved + each document's readable html + hub.
- **Both** — the above + the diagram dashboard `overview.html` (`html-visual-template.md`).
- **Visual dashboard only** — `index.html` = diagram dashboard only (`html-visual-template.md`).
- **None (md only)** — skip HTML.

Freeze the choice into `spec.md` / `sprint-playbook.md`. The default suggestion is "Per-document HTML + hub."

## Output structure (per-document HTML + hub)

```
docs/                         (Mode 5 uses docs/; Modes 1–4 use a single folder)
├─ index.html                     hub: document list, importance tiers, work order, link to each document
├─ 00-business-model.md  / .html  original md preserved + readable html (Mode 5 example)
├─ 01-service-plan.md    / .html
├─ ... (a same-named .html next to each .md)
├─ INDEX.md                        text guide (source of the hub index.html)
└─ overview.html                   (only when "Both" is selected) diagram-rich dashboard
```
Modes 1–4 (single document) produce only `service-plan.md` + `service-plan.html` (no hub needed, though generate one if desired).

## Technical policy (self-contained, one file each)

- Each html has **zero external dependencies** (inline CSS/JS/SVG only; no CDN, fonts, plugins, or Mermaid). Opens on double-click; OK for offline use, email attachment, and intranet sharing. (Generated images are local files via `assets/` relative paths.)
- System fonts. A restrained palette with one accent. Body readability first (max-width ~820px, line-height 1.7).
- **No new content** — faithfully convert the `.md` body to HTML (no summarizing, deleting, or inventing). But **visualizing existing data is allowed and encouraged**: preserve ASCII diagrams in `<pre>`, promote tables/trees/flows/matrices into **inline SVG/CSS infographics** with the same numbers and relationships (no inventing new data), and display generated images (`assets/`). Because it's infographic-first, don't leave it as a wall of text.

## Per-document readable HTML (`NN-name.html`) rules

- Top bar: document title + document number/tier badge + `[← List]` (index.html) + `[← Prev / Next →]` document links + `[🖨 Print/PDF]`.
- Body: md → HTML faithful conversion. Headings → h2/h3/h4 (anchor id), tables → styled tables (horizontal-scroll wrapper), lists, code → monospace blocks, quotes → callouts. Code/ID tokens (`F1`·`SC-02`·`P3`) as monospace chips.
- **Visualization (infographic-first)**: preserve the md's ASCII diagrams (trees, flows) in `<pre>`. Promote structural data (IA tree, user flow, competition/permission matrix, ERD, Gantt, ecosystem map, metrics) into **inline SVG/CSS infographics** with the same numbers and relationships, shown alongside (or instead of) the ASCII. Generated images (`assets/…`) are displayed via `<img>`. **No external renderers or CDNs such as Mermaid** — everything is inline SVG/CSS.
- Right/left **TOC**: the document's headings, highlight the current position while scrolling (IntersectionObserver), click to jump.
- **Review convenience**: each h2/h3 has `#anchor` (share a URL to a specific section), with a § link next to the section. (Optional) show the anchor on paragraph hover.
- Single-source caution: money rules and the like only cite `00-business-model` (no redefining) — same as the md.
- Print CSS: hide top bar, buttons, and TOC; body full-width; page-break-inside avoid.
- Accessibility: semantic landmarks, contrast, focus.

## Hub (`index.html`) rules

This is a **navigation page that directly visualizes `INDEX.md`** (importance grouping). Don't create new categories; follow INDEX.md.
- Header: service name + one-line summary + meta (date written, document count).
- **Cards per importance tier**: T1 essential / T2 conditional / T3 supplementary — each tier has document cards (number, name, one-line purpose, `NN-name.html` link, tier badge).
- **Work order**: a numbered reading/writing order (each item links to the corresponding html).
- **Per-role bundles**: Dev/QA/Designer/Planner/Management tabs or sections — link only the documents each role should see.
- **Minimal start set**: a highlighted box + links to those documents.
- (When "Both" is selected) a `[📊 View diagram dashboard → overview.html]` button at the top.
- Self-contained (inline CSS/JS), print CSS, responsive (single-column cards on narrow screens).

## Skeleton (per-document html — fill the slots)

```html
<!doctype html><html lang="ko"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{DOC_TITLE}} · {{SERVICE}}</title>
<style>/* variables, typography, tables (zebra, wrapper), chips, callout, TOC, @media print (hide top bar/TOC), responsive */</style></head>
<body>
<header class="topbar">{{DOC_NO}} {{TIER_BADGE}} · {{DOC_TITLE}}
  <nav><a href="index.html">← List</a> <a href="{{PREV}}">← Prev</a> <a href="{{NEXT}}">Next →</a>
  <button onclick="window.print()">🖨 Print/PDF</button></nav></header>
<div class="layout"><nav class="toc">{{TOC}}</nav>
  <main>{{BODY_HTML}}</main></div>
<script>/* IntersectionObserver TOC highlight, smooth scroll, heading anchors */</script>
</body></html>
```
> Note: `lang="ko"` is a default; set it to match the chosen output language (deliverable language is user-selectable, default Korean — the HTML just renders whatever language the .md is in).

## Rules (check during audit)

- **md preserved**: every `.md` must remain intact (no deleting or altering). HTML is additive.
- **Faithful conversion**: the html body holds the md content with no omission, summarizing, or invention. Tables, code, lists, and ASCII diagrams preserved.
- **Infographic-first**: structural data is visualized as inline SVG/CSS infographics (zero empty diagrams or invented data). Not a wall of text.
- **Self-contained**: zero external dependencies (no CDN, Mermaid, or fonts), opens on double-click. Images are local in `assets/`.
- **Hub = reflects INDEX.md**: tiers, order, roles, and minimal set match INDEX.md; all links valid (relative paths).
- **Print/accessibility/responsive** satisfied.
- Modes 1–4 produce a single-document html; Mode 5 produces per-document html + hub (+ optional overview.html).

## References

- For the diagram-rich dashboard see `html-visual-template.md` (the `overview.html` of "Both").
- The source of the importance grouping is the "INDEX.md generation rules" in `mode-templates.md`.
