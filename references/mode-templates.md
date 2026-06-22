# Mode Templates — service-planning-harness

> Document structures (section skeletons) for 4 deliverable modes. Based on the mode the user picks in STEP 1, the Planner fills the "Structure / flow" section of `spec.md` with this skeleton, and during S4 integration the Generator writes `service-plan.md` in this structure.
> Non-functional requirements shared by all modes: 0 placeholders/TBDs, every core feature maps to a specific problem from the problem definition, every success metric includes a measurable number + measurement method, MVP/out-of-scope explicitly separated, **every document with structure embeds concept-fit visualization (infographic-first, G-g)**. (§8 gates G-a–G-g)

> Section labels here are English; the Generator renders headings in the user-selected output language (STEP 1-c; default Korean).

---

## Infographic-first — per-document optimal visual-representation mapping (shared by all modes)

> Content that has structure (hierarchy / flow / relationships / time axis / comparison / state transition) is delivered not as a text wall but as **the visual representation that best captures that concept**. Three tiers of visualization production (use the highest available): ① **image generation (when possible)** real illustrations (webtoon / ecosystem / concept) → `docs/assets/`, referenced via md `![](assets/…)` / HTML / ② **inline SVG/CSS infographics** (the primary medium for HTML deliverables, 0 external dependencies) / ③ **ASCII diagrams / matrix tables** (md body, no renderer needed). **Diagrams that depend on external renderers such as Mermaid are not recommended** (self-contained, offline, design control). When a standard diagram does not fit the concept, **invent a topic-specific visualization**.

| Document | Core structure | Optimal visual representation (md: ASCII/matrix · HTML: inline SVG · image when possible) |
|------|-----------|----------------------------------------------------------------|
| 00-business-model | Money/settlement flow / unit economics / triggers | Money-flow diagram (Sankey-style SVG/ASCII) · settlement trigger sequence · unit-economics bar/donut (SVG) / table |
| 01-service-plan | Overall structure / value loop / MVP scope | Structure mind map · value-loop diagram · MVP↔Phase2 2-axis/quadrant |
| 02-market-competition | **Ecosystem / participants** / positioning / competitive comparison / segmentation | **Ecosystem map (stakeholder / value-network diagram)** · positioning quadrant (2-axis scatter) · comparison matrix (✓/△/✗ color chips) · segmentation donut/pie |
| 03-personas | Journey / value loop / empathy | Persona journey map (journey) · **explainer webtoon panels (image when possible)** · value loop |
| 10-prd | Feature hierarchy/priority / scope | Feature mind map (P0/P1/P2 branches/colors) · scope quadrant |
| 11-user-stories | Epic→Story hierarchy | Epic-Story tree (mind map/graph) |
| 12-ia | Screen hierarchy tree | IA tree diagram (color by role, ASCII→SVG) |
| 13-user-flows | Paths/branches / key sessions | User-flow flowchart · key-session sequence/state diagram |
| 20-wireframes | Low-fidelity layout | ASCII layout blocks (visual craft = STEP 7 human / mockup image when possible) |
| 21-screen-spec | Screen state transitions | State diagram (empty/loading/error/normal transitions) · per-screen state table |
| 30-functional-spec | Input→processing→output / branches | Processing flowchart (validation / exception branches) |
| 31-erd | Entities / relationships | ERD diagram (entity boxes + 1:N relationship lines, SVG/ASCII) |
| 32-api-spec | Request/response flow | API sequence diagram (client→API→DB→external) · endpoint table |
| 33-policy | Permission matrix / settlement-refund-consent flows | Permission-matrix heatmap · refund/dispute flowchart · consent sequence |
| 40-backlog | Schedule / critical path | Gantt chart (sprint allocation / critical-path emphasis, SVG) |
| 41-qa-testcases | Coverage / case map | Test-coverage map (mind map) · case table |

> Modes 1–4 (single document) use only what fits the relevant sections (e.g. Mode 1 Lean → structure mind map · value loop · MVP quadrant · key-flow flowchart · metric charts · comparison matrix). Mode 5 uses all of the above. In any mode, **a section that has structure is never left as prose/tables only without a diagram (G-g)**.
>
> **Always-on visual rule for UI/screens/design (mandatory)**: Everywhere UI/GUI/UX, screen design, or design is mentioned (12-ia · 13-user-flows · 20-wireframes · 21-screen-spec, Mode 2 screen definition, Mode 4 per-screen spec, etc.), do not stop at text description — **always** accompany it with a visual (wireframe · screen layout · state-transition diagram · flow · component diagram · mockup image when possible). Describing a screen in prose alone = G-g FAIL.

---

## Mode 1 — Lean MVP plan [default/recommended]

The lightest, fastest-to-start minimal plan. Scoped so a dev team can build it within 4 weeks.

```
1. Problem definition (who-what-why)
   - For whom / what / why is it a problem
2. Target (1 key persona)
   - 1 person including situation / context / pain points (1 key persona because Lean)
3. Core features
   - each feature → problem mapping (feature↔problem traceability table)
4. User flow
   - key flow only (no wireframes)
5. MVP scope
   - MVP included / Out-of-scope (explicitly excluded) separated
   - priority + rationale
6. Success metrics
   - each metric: number + measurement tool + period + decision criteria
7. Differentiation (brief)
   - explicit comparison against at least 1 real competitor/alternative
```

S3 light (key user flow only, no wireframes) → STEP 7 checkpoint generally skipped.

---

## Mode 2 — Formal PRD

Mode 1 structure + functional spec / screen definition / edge cases / non-functional requirements. A formal product requirements document.

```
1. Problem definition (who-what-why)
2. Target (personas, multiple if needed)
3. Core features + functional spec
   - each feature → problem mapping (traceability table)
   - detailed spec per feature (input/processing/output/constraints)
4. Screen definition
   - per-screen purpose / components (wireframes possible → subject to STEP 7 gate)
5. User flow
   - normal flow + edge cases
6. Edge cases / exception handling
7. Non-functional requirements
   - performance / security / accessibility / scalability, etc. (qualitative/quantitative criteria)
8. MVP scope (MVP included / Out-of-scope separated, priority + rationale)
9. Differentiation (explicit comparison against at least 1 competitor + comparison axes)
10. Success metrics (each metric: number + measurement tool + period + decision criteria)
```

Wireframes/mockups may be included in screen definition → if included, STEP 7 human checkpoint is triggered.

---

## Mode 3 — Combined business + planning document

Mode 2 structure + market / competition / revenue-model business-analysis sections. S1 is expanded (market size / segments / revenue model).

```
1. Executive Summary (1-page summary)
2. Market analysis
   - market size (TAM/SAM/SOM rationale), segments, trends
3. Ecosystem analysis (ecosystem)
   - all ecosystem participants (supply/demand side, platform/intermediary, complements/substitutes, regulators/institutions, payment/infrastructure/data partners) + each one's role / incentive / value exchange / dependency
   - **ecosystem map (stakeholder / value-network diagram)** — visualize value/money/data flows among participants
4. Competitive analysis
   - multiple real competitors + comparison-axis matrix, moat/differentiation argument
5. Revenue model
   - pricing structure, unit-price / unit-economics assumptions, revenue hypotheses
6. Problem definition (who-what-why)
7. Target (personas)
8. Core features + functional spec (feature↔problem traceability table)
9. User flow / screen definition
10. MVP scope (MVP included / Out-of-scope separated, priority + rationale)
11. Differentiation (explicit competitor comparison + comparison axes + moat)
12. Success metrics (business KPIs + product KPIs, each metric number + measurement method)
13. Risks & assumptions
```

S1 expanded (market / revenue model). Wireframes are usually non-core → STEP 7 usually skipped (triggered if included).

---

## Mode 4 — Screen/feature-spec focused

Centered on user flow / per-screen features / data structure. S3 is the heaviest (per-screen detail + data structure + wireframes).

```
1. Problem definition (brief — who-what-why, compressed since spec-focused)
2. Target (key persona)
3. Information architecture (IA) / screen list
4. User flow (overall flow diagram level)
5. Per-screen detailed spec
   - per screen: purpose / components / interaction / state (empty/loading/error) / edge cases
   - wireframe/mockup blocks (→ subject to STEP 7 human checkpoint)
6. Data structure
   - main entities / fields / relationships (conceptual level; implementation stack is owned by the Generator but the spec does not force it)
7. Core feature → problem mapping traceability table
8. MVP scope (MVP included / Out-of-scope separated)
9. Differentiation (explicit comparison against at least 1 competitor)
10. Success metrics (each metric number + measurement method)
```

S3 heaviest (per-screen detail + data structure + wireframes) → STEP 7 human checkpoint almost always triggered.

---

## Mode 5 — Full Planning Package

Generates the **entire set of planning deliverables** for an IT service development project in one go. Not a single document but numbered multiple documents in a `docs/` directory. After sharing S1–S3 research, S4 expands into **per-deliverable-group generation**.

> The same non-functional requirements shared by all deliverables apply (§8 gates G-a–G-g, including infographic-first G-g). **Single-source-of-truth rule**: money rules such as revenue / settlement / pricing are finalized in one place, `00-business-model.md`, and downstream documents (ERD / API / policy) reference it as-is (no duplicate definition / contradiction). All documents must be mutually traceable (feature↔problem↔screen↔data↔API consistent).

### Deliverable list (5 stages, ~16 types)

```
docs/
─ Discovery / strategy (why are we building it)
  00-business-model.md       revenue sources / fees / settlement triggers / unit economics (single source)
  01-service-plan.md         service-planning document (problem / target / core value / MVP scope / metrics / differentiation)
  02-market-competition.md   market / competition analysis (organized from S1)
  03-personas.md             key personas (based on S2)
─ Definition (what are we building)
  10-prd.md                  PRD — feature list / priority / scope / non-functional requirements
  11-user-stories.md         user stories / requirements spec ("as a ~, I want to ~" + acceptance criteria)
  12-ia.md                   information architecture (IA) — screen/menu hierarchy tree
  13-user-flows.md           user flow / user journey (main path steps)
─ Design (how it looks)
  20-wireframes.md           wireframes (low-fidelity text/ASCII — subject to STEP 7 human checkpoint)
  21-screen-spec.md          screen spec / storyboard — per-screen feature/behavior/state/exception (directly tied to dev)
─ Technical (development spec)
  30-functional-spec.md      functional spec — per-feature input/processing/output/constraints
  31-erd.md                  ERD / data model — entities / fields / relationships (conceptual~logical level)
  32-api-spec.md             API spec — endpoints / request-response / errors
  33-policy.md               policy — permissions / terms / billing-settlement / exception rules (references 00)
─ Execution / verification
  40-backlog.md              backlog / sprint plan — dev units / priority / schedule
  41-qa-testcases.md         QA test cases — key scenarios / expected results
─ Guide + HTML (STEP 1-b choice; md always preserved)
  INDEX.md                   importance grouping + work order + per-role bundles + minimal start set across all deliverables (generated last, after all documents are written).
  index.html                 [if per-doc HTML + hub chosen] hub — importance / order / per-role links (reflecting INDEX.md). `references/html-doc-template.md`.
  00-…/01-… .html            [if per-doc HTML + hub chosen] each `.md`'s read/share/review HTML (original .md preserved). `references/html-doc-template.md`.
  overview.html              [if "both" chosen] aggregate visual dashboard (mind maps / charts / infographics / webtoon). `references/html-visual-template.md`.
  (or) index.html            [if "visual dashboard only" chosen] index.html = aggregate visual dashboard.
```

### INDEX.md generation rule (last, after all documents are written — g7)

After making all 16 types, generate `INDEX.md` so the reader knows "what to read first / who / why". To avoid becoming a document for documents' sake, **judge actual necessity per mode/domain** and group accordingly. Include these 4 blocks:

1. **3 importance tiers** (place each document in one + one-line rationale):
   - **T1 essential** — can't start development without it: `10-prd` · `13-user-flows` · `21-screen-spec` · `31-erd` · `32-api-spec` · `40-backlog` · `01-service-plan`.
   - **T2 conditionally essential** — essential depending on domain: `00-business-model` (billing/marketplace/SaaS) · `33-policy` (payment / personal data / regulation) · `41-qa-testcases` · `30-functional-spec`. (If essential for this service, promote to T1 in the notation.)
   - **T3 supporting** — for alignment / persuasion / onboarding: `02-market-competition` · `03-personas` · `11-user-stories` · `12-ia` · `20-wireframes`.
   - The judgment is not fixed. E.g.: for a payment service make `33-policy` · `00-business-model` T1; for an internal admin tool omit `02` · `03` or move to T3. INDEX records the actual tiers for this service.

2. **Work order** (dependency-based, numbered): ① `00` (fix the money rules single-source first) → ② `01` · `02` · `03` (why/who) → ③ `10`→`11` (what) → ④ `12`→`13`→`20`→`21` (how it looks) → ⑤ `30`→`31`→`32`→`33` (how it works, referencing `00`) → ⑥ `40`→`41` (execution / verification) → ⑦ `index.html` (visual aggregate). If dependencies change, adjust the order.

3. **Per-role bundles** (no single person reads all 16 types):
   - Developer: `21` · `31` · `32` · `30` · `40`
   - QA: `11` (acceptance criteria) · `41` · `13`
   - Designer: `12` · `13` · `20` · `21` · `03`
   - Planning/PO: `01` · `10` · `40`
   - Management/investors: `00` · `02` · `03`

4. **Minimal start set** (Lean — enough to start development with just these): `10-prd` · `13-user-flows` · `21-screen-spec` · `31-erd` · `32-api-spec` · `40-backlog`. + add `00` if billing-type. State that "the full set is for kickoff / delivery / regulation, and this set is enough day-to-day".

Attach a one-line purpose next to each document name, plus a T1/T2/T3 badge. INDEX.md must not generate new content — it only classifies/sorts existing deliverables.

### Skeleton of each deliverable (gist)

> Each document embeds the optimal visualization from the "infographic-first mapping" above into the body (G-g). md = ASCII diagrams / matrices, HTML = inline SVG, generated image when possible. Mermaid not recommended.

- **00 business-model**: revenue-sources table / **money-flow diagram** / take-rate justification / unit economics (example calculation + bar/donut) / **settlement trigger sequence** (event→who→how much→when) / phase breakdown / assumptions / risks.
- **01 service-plan**: Mode 1's 7 sections (problem / target / features / flow / MVP scope / metrics / differentiation) + revenue-model section (link to 00) (**structure mind map** + **value loop** + MVP↔Phase2 quadrant).
- **02 market-competition**: **ecosystem analysis (all participants / roles / incentives / value exchange / dependencies) + ecosystem map (stakeholder / value-network diagram)** + named-competitor matrix (✓/△/✗) + positioning quadrant + comparison axes + market context + gaps/opportunities.
- **03 personas**: supply/demand key personas (situation / pain points / frequency / outcome) + **persona journey map** + value loop + (when possible) **explainer webtoon panel image**.
- **10 prd**: feature ID list + priority (P0/P1/P2) + summary per feature + non-functional requirements (performance / security / accessibility) + scope/out-of-scope (**feature mind map** priority colors + scope quadrant).
- **11 user-stories**: Epic→Story hierarchy (**Epic-Story tree**), each story with acceptance criteria (Given-When-Then) + linked feature ID.
- **12 ia**: screen/menu hierarchy tree (**IA tree diagram**, color by role) + screen ID assignment + exposure by permission.
- **13 user-flows**: main flows (by role) steps + branches + each step linked to screen ID / feature ID (**user-flow flowchart** + key-session **sequence/state diagram**).
- **20 wireframes**: low-fidelity layout of key screens (text/ASCII blocks, mockup image when possible) — **visual craft is an LLM limitation, so it needs user review/approval at STEP 7**.
- **21 screen-spec**: per screen — purpose / components / interaction / state (empty / loading / error) / exception / linked feature / API (**state-transition diagram** + per-screen state table).
- **30 functional-spec**: per-feature input / processing / output / constraints / validation rules / exception handling (**processing flowchart** — validation / exception branches).
- **31 erd**: entity list + fields (type / constraint) + relationships (1:N, etc.) + main index candidates. **ERD diagram (entity boxes + relationship lines, inline SVG / md is ASCII)** + table (Mermaid not recommended).
- **32 api-spec**: per-resource endpoints (method / path) + request/response schema + status codes / errors + auth (**API sequence diagram** client→API→DB→external + endpoint table).
- **33 policy**: permission matrix (**heatmap**) / billing-settlement rules (references 00) / key terms / refund-dispute (**flowchart**) / consent flow (**sequence**) / moderation / data-privacy.
- **40 backlog**: Epic→sprint allocation (weekly, assuming a 4-week MVP) + dependencies + estimation (high/medium/low) (**Gantt chart** — sprint allocation / critical-path emphasis).
- **41 qa-testcases**: test cases per key flow (precondition / input / expected result / priority) + edge / exception (**test-coverage map** — mind map).

S3 is heavy (IA / flows / screens), and S4 expands into the 16 types above. Includes 20-wireframes → **STEP 7 human checkpoint almost always triggered.**

---

## Per-mode sprint-scope adaptation summary

| Mode | S1 (market / competition / ecosystem) | S2 (problem / target) | S3 (UX/UI flow) | S4 (integration) | STEP 7 wireframe gate |
|------|----------------|----------------|-------------------|-----------|-----------------------------|
| 1 Lean MVP | at least 1 explicit competitor + ecosystem participants/map | 1 key persona | light — key flow only, no wireframes | Mode 1 structure | usually skipped |
| 2 Formal PRD | at least 1 explicit competitor + ecosystem map | personas (multiple possible) | + screen definition + edge cases | Mode 2 structure | triggered if wireframes included in screen definition |
| 3 Business + planning | expanded — market size / segments / revenue model + ecosystem analysis/map | personas | standard flow + screens | Mode 3 structure | usually skipped (triggered if included) |
| 4 Screen/feature spec | at least 1 explicit competitor + ecosystem map | key persona | heaviest — per-screen detail + data structure + wireframes | Mode 4 structure | almost always triggered |
| 5 Full Planning Package | expanded (comparison matrix + ecosystem analysis/map → 02 document) | multiple personas (→ 03 document) | heavy (IA / flow / screens → 12 · 13 · 20 · 21) | **multiple documents ~16 types** (S4 expands per deliverable group) | almost always triggered (20-wireframes) |
