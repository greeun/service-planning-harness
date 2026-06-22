---
name: service-planning-harness
description: A **3-agent GAN harness skill** (Planner→Generator→Evaluator, Full tier) that takes a short service idea or set of requirements and produces a **development-ready service-planning document (or a full planning-deliverable package)**. On activation it first has the user pick one of 5 output modes (Lean MVP plan / Full PRD / Business+planning combined / Screen·feature-spec focused / Full planning package), then runs 4 research sprints (S1 market·competition research → S2 problem·target → S3 UX/UI flows → S4 plan integration) with per-sprint contracts and quality gates, adapting the document structure to the chosen mode, while a separate Evaluator adversarially verifies each sprint + a final 4-axis rubric. The full planning package (Mode 5) generates a 5-stage ~16-deliverable set (plan·PRD·IA·user flows·screen spec·functional spec·ERD·API spec·policy·backlog·QA, etc.) in one pass. **Infographic-first**: every deliverable conveys structure through the best-fit visual (mind map·flowchart·sequence·state diagram·ERD·Gantt·comparison matrix·chart·journey map·ecosystem map·infographic·webtoon — or a topic-specific visualization invented when no standard diagram fits) rather than a wall of text — embedded as ASCII diagrams·matrices in the `.md` body and as data-driven **inline SVG/CSS infographics** in the HTML output, with a diagram-rich HTML dashboard produced by default (self-contained·offline — external-renderer diagrams such as Mermaid are discouraged). Use when asked: "서비스 기획", "기획서 작성", "서비스 기획서", "PRD 작성", "MVP 기획", "제품 기획", "서비스 기획 스킬", "화면 명세 작성", "전체 기획 산출물", "기획 패키지"; English: "service planning", "write PRD", "product spec", "MVP plan", "full planning package". (14 triggers: 10 Korean + 5 English. Skill instructions are English; the generated deliverable's language is user-selectable at STEP 1-c, default Korean.)
---

# Service Planning Harness

A GAN-style 3-agent harness that takes a short service idea (1–4 sentences) and produces a **service-planning document** an engineering team can start from immediately. Planner → Generator → Evaluator are each dispatched as separate `Agent` calls (GAN anti-omission-bias). **Full tier**: it runs 4 research sprints with per-sprint contract negotiation + quality gates, and runs per-sprint + a final integrated Evaluator pass.

> Skill-authoring language is English. The **generated deliverable** is written in the user-selected output language (STEP 1-c; default Korean). Korean trigger phrases are intentional — the skill must still activate on Korean requests.

## Core design intent (why Full tier)

Service planning requires **research-phase decomposition + per-phase quality gating**. Stacking a plan on top of weak market research / problem definition makes differentiation (C2) and feasibility (C3) collapse structurally. So S4 synthesizes the plan **only after** the Evaluator passes the S1–S3 research through gates. This gating is not for context relief — it is an orthogonal quality mechanism. (See the model-guidance section — Opus 4.8 has effectively no context anxiety, yet Full was chosen deliberately.)

## Reference files (references/)

| File | Role |
|------|------|
| `references/planner-prompt.md` | Planner prompt — handle mode selection + write spec.md/sprint-playbook.md |
| `references/generator-prompt.md` | Generator prompt — per-sprint contract negotiation + production + self-verify |
| `references/evaluator-prompt.md` | Evaluator prompt — per-sprint + final pass, 8 probes, verdict logic |
| `references/rubric.md` | 4 criteria (C1–C4), weights (C2/C3 2×), justification, verdict logic |
| `references/evaluator-calibration.md` | few-shot 1/3/5 anchors (C1–C4) — scoring calibration |
| `references/sprint-playbook.md` | 4-sprint plan, deliverables, mode-adapted scope, observable checks |
| `references/mode-templates.md` | document-structure skeletons for the 5 output modes (incl. Mode 5 full-package 16 deliverables) |
| `references/html-doc-template.md` | render each deliverable `.md` as readable/shareable/reviewable HTML + hub `index.html` (nav by importance·order·role). ASCII preserved + inline SVG infographics + generated images (no external-renderer dependency). md preserved. Included in the STEP 1-b default ("Both") |
| `references/html-visual-template.md` | render the final deliverables as a self-contained, diagram-rich HTML dashboard (mind map·diagrams·flowcharts·sequence·ecosystem map·architecture·charts·infographics·webtoon + topic-specific invented visualizations), all **inline SVG/CSS + generated images where available** (Mermaid discouraged). The `overview.html` of the default "Both" / the `index.html` of "Dashboard only" |

## 5 output modes

| Mode | Name | Gist |
|------|------|------|
| 1 | Lean MVP plan [default/recommended] | Minimal plan buildable in ~4 weeks. Light S3, no wireframes. |
| 2 | Full PRD | + functional spec / screen definitions / edge cases / non-functional requirements. |
| 3 | Business + planning combined | + market / competition / ecosystem / revenue-model analysis. Expanded S1. |
| 4 | Screen·feature-spec focused | Centered on user flows / per-screen features / data structures. Heaviest S3 + wireframes. |
| 5 | Full planning package | A 5-stage ~16-deliverable set (discovery·definition·design·technical·execution) + a **final visual HTML dashboard (`index.html`)**. After S1–S3 research, S4 expands into multi-document generation (g1~g6). Not a single service-plan.md but a numbered document set under `docs/` + diagram-rich visualization HTML. |

For detailed section structures see `references/mode-templates.md`. Mode 5's deliverable list and document skeletons are defined there too.

## 4 research sprints (fixed, scope variable)

| Sprint | Name | Deliverable | Feeds | Mode adaptation |
|--------|------|-------------|-------|-----------------|
| S1 | research / market·competition·ecosystem | `research-s1.md` | C2, G-c/G-g | ecosystem+participant description·ecosystem map always included; Mode 3 expanded (market/revenue model); otherwise at least 1 named competitor |
| S2 | problem·target·persona | `research-s2.md` | C1, G-a/G-b | Mode 1 lean (1 persona); others may have multiple |
| S3 | UX/UI flows·screens | `research-s3.md` | P-1, G-a/G-g | Mode 1 light (no wireframes); Mode 4 heaviest |
| S4 | plan integration | `service-plan.md` | C3, G-a/G-d/G-e/G-g | integrate S1–S3 into the chosen mode's structure (no new research) |

The 4 sprints **always** run; only each sprint's scope adapts to the mode. Details in `references/sprint-playbook.md`.

---

## Activation Flow (orchestrator)

An 8-step flow. STEP 1 = output-mode selection (mandatory gate). The Full-tier 4-sprint loop (STEP 4) has per-sprint contract negotiation + evaluation.

### STEP 1 — mode selection + HTML form + output language (mandatory gate)
On activation, ask the user for and receive **three** things together:

**(1-a) Output mode** — (1) Lean MVP plan [default/recommended], (2) Full PRD, (3) Business+planning combined, (4) Screen·feature-spec focused, (5) Full planning package (5-stage ~16 deliverables). If unanswered/ambiguous, **propose** (1) Lean MVP but do not generate before confirmation.

**(1-b) HTML output form** (md always preserved; infographic-first, so a diagram dashboard is included by default; `references/html-doc-template.md`):
- **Both (per-document HTML + diagram-rich dashboard)** [default/recommended] — render each `.md` as readable/shareable/reviewable html (body ASCII diagrams preserved + data-driven inline SVG infographics·generated images shown) + hub `index.html` (links by importance·order·role, reflecting `INDEX.md`) + diagram-rich dashboard `overview.html` (`references/html-visual-template.md`).
- **Visual dashboard only** — `index.html` = diagram-rich dashboard only (`references/html-visual-template.md`).
- **Per-document HTML only** — each `.md` → readable html (ASCII diagrams·inline SVG·generated images) + hub, dashboard omitted.
- **None (md only)** — skip HTML. (But the md body's infographics/diagrams [ASCII·matrices·generated images where available] are always embedded — G-g.)

**(1-c) Deliverable output language** — the language the generated deliverable is written in: **Korean [default]** or **English**. (The skill's own instructions are English; this controls only the produced document's prose/section headings.) If unanswered, default to Korean.

Freeze the chosen mode, HTML form, and output language into `spec.md` / `sprint-playbook.md`. **Output-location convention: all deliverables are written under the project's `docs/`** — Mode 5 → `docs/` (numbered 16 `.md` + optional HTML + `INDEX.md`), Modes 1–4 → `docs/` (`service-plan.md` + optional HTML). Harness intermediate files (`spec.md`·`sprint_contract.md`·`research-s*.md`·`critique.md`·`handoff.md`) live in the same working tree (`docs/` or a sub-working folder). **Mode 5 is multi-deliverable**, so STEP 4 S4 expands per deliverable-group and the STEP 7 wireframe gate almost always fires (screen spec included).

### STEP 2 — intake
Receive the 1–4 sentence service idea/requirements. If too vague, ask back about only 1–2 essentials and proceed (no over-questioning). If still vague, the Planner forms the single sharpest hypothesis and records it as an assumption in spec/playbook.

### STEP 3 — dispatch Planner
Run the Planner once via an `Agent` call on `references/planner-prompt.md`. It writes `spec.md` with the frozen mode's section structure and **writes the 4-sprint plan + each sprint's mode-adapted scope into `sprint-playbook.md`**. Returns: `SPEC_READY: spec.md` + `PLAYBOOK_READY: sprint-playbook.md`.

### STEP 4 — 4-sprint loop (S1→S2→S3→S4, each sprint cap 5–15)
For each sprint in order:

- **4a — contract negotiation**: The Generator (`Agent` call, `references/generator-prompt.md`) proposes that sprint's `sprint_contract.md` (scope + observable checks + output file/format). Dispatch the Evaluator (`Agent` call, `references/evaluator-prompt.md`, Mode A1) to **approve/amend** the contract. If the checks are weak, the Evaluator strengthens them. **No build before approval (safety gate).**
- **4b — sprint build**: Dispatch the Generator via an `Agent` call. It produces that sprint's deliverable (`research-s1.md`/`-s2.md`/`-s3.md`, or S4's `service-plan.md`) and `generator_report.md`, self-verifying before handoff. Returns: `READY_FOR_QA`, or `HANDOFF_NEEDED` (→ 4d), or `DEADLOCK` (→ STEP 8).
- **4c — sprint evaluation**: Dispatch the Evaluator (`Agent` call, Mode A2) to produce `critique.md` against that sprint's observable checks + sprint-relevant probes/gates. On `PASS`, advance to the next sprint; on `FAIL` and round < cap, retry the same sprint (applying the Generator's Strategic Decision: REFINE/PIVOT); on `FAIL` and round = cap, transparently record the unresolved issues and get the user's judgment on whether to advance.
- **4d — context handoff**: On `HANDOFF_NEEDED`, resume the sprint with a fresh Generator session reading only `handoff.md`. **No compaction** — compaction preserves the anxiety state, so it is forbidden as a recovery path (reset ≠ compaction).

### STEP 5 — final integrated Evaluator pass
After S4, dispatch the Evaluator (Mode B) via an `Agent` call to run **the 4-axis rubric + 8 adversarial probes + all §8 7 gates** on the integrated `service-plan.md`, producing `critique.md`. Returns: `CRITIQUE_READY: critique.md`.

### STEP 6 — verdict branching
- `PASS` (all ≥4, 2×(C2/C3) ≥4, probes clean, all §8 pass) → STEP 7.
- `FAIL` → if the gap is attributable to a specific sprint deliverable, return to that sprint (STEP 4, within cap); if it's an integration-stage gap, retry S4. If the overall cap is reached, transparently report the current best version + unresolved blocking issues.

### STEP 7 — conditional wireframe human-checkpoint (P-1, most relevant right after S3)
Check whether `research-s3.md` / `service-plan.md` contains a **wireframe / screen-mockup block**.
- **Not present (Modes 1/3 generally)** → **skip** the checkpoint, fully automatic, proceed to STEP 8.
- **Present (Modes 2/4 possible)** → **ask the user to review·approve the wireframes**, and do not confirm the final PASS before approval. The text part PASSes normally; the visual part is marked `HUMAN_CHECKPOINT_REQUIRED` and surfaced to the user.

> Rationale for this gate: an LLM can read every word of a text deliverable and judge it (sensory limits = none), but the visual craft of a UI wireframe/mockup exceeds the LLM's reliable aesthetic judgment. So a human checkpoint is inserted only in that case. "The Evaluator will handle it" is not allowed — visual approval is a human's.

### STEP 7.5 — HTML render (branches on the STEP 1-b choice; md always preserved)
Render the PASSed final deliverable into HTML in the form chosen at STEP 1-b. **All HTML generates no new content — only faithfully convert/visualize the existing `.md` deliverables** (money rules cite the single source). Readable HTML preserves the md's ASCII diagrams (monospace), promotes md data into **inline SVG/CSS infographics**, and displays generated image assets (`docs/assets/`). **No external-renderer dependency (Mermaid discouraged) — every visualization is self-contained.** If wireframes / visual craft are included, render after the STEP 7 human checkpoint clears.
- **Both** [default]: per `references/html-doc-template.md`, each `.md` → readable `NN-name.html` (ASCII preserved + inline SVG infographics + generated images; +Modes 1–4 get `<service>-plan.html`) + hub `index.html` (nav from `INDEX.md`'s importance·order·role) **+ diagram-rich dashboard `overview.html`** (`references/html-visual-template.md`). Both readability·sharing·review + structure-at-a-glance.
- **Visual dashboard only**: `index.html` = diagram dashboard (`references/html-visual-template.md`). Fill the visualization catalog (mind map·value loop·ecosystem map·money flow·competition matrix·persona webtoon·user flow·sequence·IA tree·architecture·ERD·metric charts·scope·Gantt, plus topic-specific invented visualizations) with real data, all inline SVG/CSS + generated images where available.
- **Per-document HTML only**: the "Both" above minus `overview.html` (each `.md` readable html [ASCII preserved + inline SVG + generated images] + hub).
- **None**: skip HTML (md only — but the md body's infographics/diagrams are always embedded).

### STEP 7.6 — importance-grouping·order guide (Mode 5, after all documents written)
In Mode 5, after all deliverables (16 + `index.html`) are complete, **generate `INDEX.md` last** (`references/mode-templates.md` "INDEX.md generation rules", sprint-playbook S4-g7). Include: ① 3-tier importance (T1 essential / T2 conditional / T3 supplementary, judged per domain) ② work order (dependency-based) ③ per-role bundles (Dev/QA/Designer/Planner/Management) ④ minimal start set (Lean 6). Don't make documentation theater — only classify·sort by actual need, no new content.

### STEP 8 — deliver/finish
Deliver the final output (Mode 5 = `docs/` 16 `.md` + `INDEX.md` + STEP 1-b HTML [per-document `NN-name.html` + hub `index.html`, or diagram `overview.html`/`index.html`]). The original `.md` is always preserved. On `DEADLOCK`, summarize both sides' positions and request the user's judgment.

### User-confirmation gate summary
(a) no generation before the STEP 1 mode is confirmed; (b) no build before the STEP 4a contract is approved; (c) no final PASS before user approval when STEP 7 wireframes are present; (d) transparent reporting of unresolved issues when the cap is reached.

### Safety gates (domain)
mode-confirmation gate (STEP 1), per-sprint contract-approval gate (STEP 4a), conditional wireframe human-checkpoint (STEP 7), §8 binary verification gates (STEP 4c/STEP 5).

---

## Iteration cap & iteration wisdom

- **iteration cap = 5–15 per sprint, default start ~8** — this is a **range, not a single value.** The cap applies per sprint (S1–S4), and the whole run is the sum of the 4 sprints.
- **Low-end (5) warning**: cutting a sprint too early (e.g. 3 rounds) can miss a **late-round breakthrough**. In one case the best result came at iteration 10 — "Dutch Art Museum leap at iteration 10". Choosing a low cap risks cutting off such late-iteration breakthroughs.
- **Wall-clock tolerance**: don't rush artificially. A good plan takes time — don't cut rounds just because progress feels slow.
- **An intermediate iteration may beat the final**: the Evaluator leaves an **Iteration Quality Note** in `critique.md`, calling out strengths a prior round had. Preserve those in the next round so they aren't lost.

---

## Principles

- **reset ≠ compaction**: under context pressure the recovery path is `handoff.md` + a fresh session, not compaction. Compaction preserves the anxiety state, so it is **forbidden** as a recovery path. Context anxiety is rare on Opus 4.8, but follow this rule when it occurs.
- **Every component encodes an assumption**: each component of this harness encodes an assumption about "something the model can't do alone." Here the sprint structure's assumption is "research quality must be gated before synthesis." On model upgrade, validate assumptions **one at a time** — remove one component at a time and measure (radical all-at-once removal failed; methodical one-at-a-time succeeded).
- **GAN separation**: Generator and Evaluator are split into separate `Agent`s precisely to structurally counter self-evaluation bias (an LLM's tendency to confidently praise mediocre work).
- **Simplicity first** (Anthropic, "Building Effective Agents"): "find the simplest solution possible, and only increase complexity when needed." This skill choosing Full is not a violation of simplicity — it's for the orthogonal need of research gating, and it simplifies once that need disappears.
- **Infographic-first**: content with structure (hierarchy·flow·relationship·timeline·comparison·state transition) is conveyed first through **the visual that best holds the concept**, not a prose/table wall — mind map·flowchart·sequence·state diagram·ERD·Gantt·comparison matrix·chart·journey map·ecosystem map·infographic·explainer webtoon, and when no standard diagram fits, **invent a topic-specific visualization** (gate G-g). Diagrams are the primary medium, not decoration. **3-tier visualization production (in priority order, use the highest available)**:
  1. **Image generation (when possible)** — if image-generation capability exists, produce real illustrations/images: persona webtoon panels·ecosystem illustrations·concept heroes·situation scenes and other assets where a picture works better. Save to `docs/assets/` and reference from md (`![](assets/…)`)·HTML. Fall back to tier 2 if unavailable.
  2. **Inline SVG/CSS infographics** — the primary visual medium for HTML output (readable + dashboard). Hand-author data-driven charts·matrices·flows·ecosystem maps·money flows·Gantt·ERD in SVG/CSS. **Zero external dependencies** (external-renderer diagrams such as Mermaid are **discouraged** — self-contained·offline·design control first).
  3. **ASCII diagrams + matrix tables** — portable visualization in the md body (no renderer needed). Trees·flows·sequences·comparison matrices. The md always carries this tier or above.
  - **Standing UI/screen/design visual rule**: **everywhere** UI/GUI/UX·screen design·design is mentioned (IA·user flows·wireframes·screen spec·screen definitions·components·states) must not end with prose explanation only and **must** be accompanied by a visual (wireframe·screen layout·state-transition diagram·flow diagram·component diagram·mockup image when possible). "Screen X shows …" as prose alone fails (G-g).
- **Ecosystem lens**: service analysis does not stop at a single user / single competitor but describes **the whole ecosystem (value network) the service sits in and all its participants** — supply/demand side, platforms·intermediaries, complement·substitute providers, regulators·institutions, payment·infrastructure partners, data/content providers, etc. Identify each participant's role·incentive·value exchange·dependency and visualize it as an **ecosystem map (stakeholder / value-network diagram)** (S1·02-market-competition). This is the foundation for the differentiation (C2)·moat·platform two-sidedness argument.

---

## V1 vs V2 guidance (per model)

This skill is **Full tier (the V1 line)**. Recommended tier by model class:

| Model class | Context anxiety | Recommended tier | Notes |
|-------------|----------------|-----------------|-------|
| **Sonnet 4.5** | Strong — wraps up prematurely | Full (V1) | Small sprints, aggressive Evaluator, firm context resets |
| **Opus 4.5** | Largely eliminated | Simplified (V2) | Multi-hour coherent sessions; sprint decomposition droppable |
| **Opus 4.6** | Eliminated; improved planning, long-context, debugging | Simplified or Single-session | 2+ hour builds sustainable; re-examine every component, drop what's not load-bearing |
| **Opus 4.8** | Eliminated (1M context) | Simplified by default — BUT this skill chooses Full deliberately | Full chosen NOT for context relief but for research-phase decomposition + per-sprint quality gating (S1–S3 research gated before S4 synthesis). Stress-test on upgrade (G-1/G-2): can the model self-organize the 4 research phases + self-gate quality without sprint contracts? If yes, drop sprints one at a time. |

General rule: every harness component encodes an assumption about what the model can't do alone. On model upgrade, stress-test each assumption individually — remove one component at a time and measure. Cite Anthropic, "Building Effective Agents" — "find the simplest solution possible, and only increase complexity when needed".

### Opus 4.8: Full vs Simplified justification (G-1/G-2)
Opus 4.8 (1M context) has effectively eliminated context anxiety — from a context standpoint the sprint structure is redundant, so the article recommends **Simplified** by default. But here **Full was chosen deliberately.** Reason: this sprint structure is not a context-relief device but an **orthogonal mechanism** — research-phase decomposition + per-phase quality gating (S4 synthesizes the plan on top of S1–S3 research only after the Evaluator passes it through gates). A strong model does not lose this gating's benefit (assumption: "research quality must be gated before synthesis" — model strength does not invalidate it).

**Upgrade stress-test (G-1/G-2, one at a time)**: "Can the model self-organize the 4 research phases and self-gate quality without explicit sprint contracts?" If YES, remove the sprint structure **one sprint/contract at a time** and re-measure — don't remove it all at once (radical removal failed; methodical one-at-a-time succeeded). The harness space doesn't shrink, it shifts — re-examine what's load-bearing on every upgrade.

---

## Evaluator tuning workflow

An untuned Evaluator is too lenient. Treat the first runs as drafts and calibrate via this **operating loop** (G-4/P-3). Don't settle for a one-sentence acknowledgment — actually run (a)–(d):

- **(a) Read critique logs** — read completed runs' `critique.md` side by side with the actual deliverables. For each rubric score, ask: "Would a demanding senior PM / planning lead give the same score?"
- **(b) Identify divergence patterns** — pinpoint the divergence patterns. Typical: lenient scoring (accepting generic/shallow output as adequate), missing unmapped features, passing non-measurable metrics, passing abstract personas on C1, judging by surface appearance without confirming substance.
- **(c) Update prompt or calibration with counter-examples** — add **concrete counter-examples** targeting that failure mode to `references/evaluator-prompt.md` or `references/evaluator-calibration.md` (e.g. "a metric that looks measurable but lacks a discrimination criterion is 3/5, not 5/5"). Few-shot calibration reduces score drift.
- **(d) Rerun on the same input; confirm the catch** — rerun on the same input and confirm the Evaluator now catches the prior miss. If it does, lock that calibration.

Skipping any of (a)–(d) is not tuning. Iterate until the Evaluator's verdicts correlate with a careful human-expert pass and every blocking issue it raises is reproducible (expect several cycles).

---

## §8 domain verification gates (binary)

All 7 binary gates must pass before the final PASS (Generator self-checks before S4 handoff; Evaluator verifies at STEP 4c/STEP 5). Details in `references/sprint-playbook.md` / `references/evaluator-prompt.md`.

| Gate | Check | PASS / FAIL |
|------|-------|-------------|
| G-a feature↔problem trace | every core feature maps to a specific problem | every feature linked to ≥1 problem / any unmapped feature |
| G-b target specificity | ≥1 persona includes situation·pain points | situation·context·pain points stated / abstract demographics only |
| G-c explicit differentiation comparison | explicit comparison vs ≥1 real competitor | competitor name + comparison axis present / unspecified or "it's better"-style |
| G-d success-metric measurability | every metric has a measurement number + method | each metric has number·tool·period / any non-measurable metric |
| G-e MVP scope / out-of-scope separation | MVP / out-of-scope explicitly separated | separation present / no separation or ambiguous |
| G-f no placeholders | placeholder/TBD/empty cells = 0 | scan finds 0 / any |
| G-g infographic fit | a document with structure (hierarchy·flow·relationship·timeline·comparison·state transition) expresses that structure as a diagram/infographic. **+ everywhere UI/GUI/UX·screen design·design is mentioned has a standing visual (required)** | each structured document embeds ≥1 concept-appropriate visualization (image/inline SVG/ASCII/matrix) **AND every UI/screen/design section has a visual (wireframe·screen layout·state-transition diagram·flow·component diagram·mockup)**, empty diagrams·placeholders = 0 / structure left as a text wall, or UI/screen/design described as prose only, or any empty diagram, or dependence on an external renderer such as Mermaid |

---

## File handoffs (file-based communication, full set)

Roles communicate only through files. Active files:
`spec.md`·`sprint-playbook.md` (Planner→) / `sprint_contract.md` (Generator⇄Evaluator, per sprint) / `research-s1.md`·`research-s2.md`·`research-s3.md` (sprint deliverables, read by later sprints) / `service-plan.md` (S4 final plan) / `generator_report.md`·`critique.md` (per sprint + final) / `handoff.md` (Generator → fresh Generator, under context pressure).

Each role is dispatched as a fresh `Agent` call reading only its handoff files. The Planner runs once; the Generator and Evaluator alternate within each sprint + across the 4 sprints.

---

## Out-of-scope
Code debugging ("fix this React component bug") and content writing ("write a blog post") are not service-planning-document production, so this skill does not activate for them. Activate only when matching the description's triggers (service planning/PRD/MVP planning/screen spec, etc.).
