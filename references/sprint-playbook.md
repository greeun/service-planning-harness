# Sprint Playbook — service-planning-harness (Full tier)

> REQUIRED Full-tier file. Defines the plan for the 4 fixed research sprints, each sprint's deliverable file, the mode-adapted scope, and the observable checks.
> In STEP 3, the Planner reflects the user-selected mode and outputs an instance of this playbook as `sprint-playbook.md` (filling in the structure below + the mode-adapted scope). At the start of each sprint the Generator reads this playbook and proposes a `sprint_contract.md`; the Evaluator approves/amends the contract against this playbook's observable bars and evaluates the sprint deliverable.

## Why 4 sprints (Full-tier justification)

A service-planning document requires **research-phase decomposition + per-phase quality gating**. If you stack a planning document on top of weak market research / problem definition, differentiation (C2) and feasibility (C3) collapse structurally. So **only after** the Evaluator passes the S1–S3 research deliverables through gates does S4 synthesize the planning document on top of them. This gating is not for context relief — it is an orthogonal quality mechanism (a stronger model does not invalidate this assumption — see the SKILL.md model guidance).

## 4 fixed sprints

In every mode the 4 sprints **always** run. Each sprint's `sprint_contract.md` scope adapts to the selected mode.

### S1 — research / market·competition research → `research-s1.md`

- **Purpose**: market context + identify at least 1 real competitor/alternative + benchmark + **ecosystem·participant analysis**. Provides the basis for differentiation (C2).
- **Feeds**: C2, §8 gates G-c (explicit differentiation comparison), G-g (ecosystem map).
- **Observable checks (bars to include in the contract)**:
  - At least 1 **real (named)** competitor/alternative is identified.
  - For each competitor, a comparison axis (what it does / does not do) is recorded.
  - Market/user context is described.
  - **The full set of ecosystem participants** (supply/demand side, platforms·intermediaries, complements·substitutes, regulators·institutions, payment·infrastructure·data partners) is identified along with their roles·incentives·value exchange·dependencies.
  - An **ecosystem map (stakeholder / value network diagram)** is visualized (infographic-first, G-g — no text wall,
    no dependence on external renderers such as Mermaid).
- **Mode-adapted scope**:
  - Mode 3 (business + planning): expanded — market sizing (TAM/SAM/SOM basis)·segments·revenue model·ecosystem analysis included.
  - Modes 1/2/4: minimal — "at least 1 explicit competitive comparison" + context + ecosystem participants·map.
- **Evaluator sprint pass**: C2 scoring + G-c/G-g gates + explicit differentiation comparison probe + ecosystem/visualization fitness probe.

### S2 — problem·target·persona research → `research-s2.md`

- **Purpose**: who-what-why problem definition + concrete persona (situation·pain points). Basis for problem-definition clarity (C1).
- **Feeds**: C1, §8 gates G-a (the problem list that is the starting point for tracing), G-b (target specificity).
- **Observable checks**:
  - The problem is sharply defined in who/what/why form.
  - At least 1 persona is concrete down to situation·context·pain points (including frequency/consequence) — abstract demographics alone fail.
- **Mode-adapted scope**:
  - Mode 1 (Lean): lean with 1 core persona.
  - Modes 2/3/4: multiple personas as needed, but the 1 core persona must be concrete.
- **Evaluator sprint pass**: C1 scoring + G-a (problem list exists)/G-b gates + target specificity probe.

### S3 — UX/UI flow·screen research → `research-s3.md`

- **Purpose**: user flows + per-screen features + (depending on mode) wireframes / screen structure.
- **Feeds**: wireframe gate (STEP 7 / P-1), G-a (feature candidates for feature↔problem tracing).
- **Observable checks**:
  - Core user flows are defined (visualized as a **user-flow flowchart** + a **sequence/state diagram** for the core session — G-g).
  - Per-screen/step features are organized (**IA tree diagram**).
  - (Depending on mode) whether wireframe / screen-structure blocks exist.
  - **Persistent UI/screen/design visuals (required)**: S3 is the core UI/UX·screen-spec sprint — screens·flows·IA must not be left as prose explanation only and must be accompanied by visuals (wireframes·layouts·state-transition diagrams·flows·component diagrams·mockup images when possible).
- **Mode-adapted scope**:
  - Mode 1 (Lean): light — core user flows only, **no wireframes**.
  - Mode 2 (PRD): + screen definitions + edge cases.
  - Mode 3 (business): standard flows + screens.
  - Mode 4 (screen·feature spec): **heaviest** — detailed per-screen features + data structures + wireframes.
- **Evaluator sprint pass**: part of C1 (flow↔problem consistency) + G-g (flow·screen diagram fitness) + when wireframes exist, mark `HUMAN_CHECKPOINT_REQUIRED` (the visual part is not scored, P-1).

### Mode 5 (full planning package) sprint adaptation

Mode 5 is not a single document but a set of ~16 deliverables. The 4 sprints run as is but with expanded scope:
- **S1**: competition matrix + **ecosystem analysis·ecosystem map** organized into `02-market-competition.md` (expanded).
- **S2**: personas into `03-personas.md` (multiple allowed). The problem list is the anchor for the PRD·user stories.
- **S3**: IA·user flows·screens·**wireframes** done heavily → research foundation for `12-ia.md`/`13-user-flows.md`/`20-wireframes.md`/`21-screen-spec.md`. Wireframes existing → triggers STEP 7.
- **S4**: expanded **per deliverable-group** (integration writing is not a single document but multiple documents per group). Recommended group split (each group = 1 sprint_contract + 1 Evaluator pass):
  - S4-g1 discovery·strategy: 00-business-model (lock the single source of truth first), 01-service-plan, 02, 03
  - S4-g2 definition: 10-prd, 11-user-stories, 12-ia, 13-user-flows
  - S4-g3 design: 20-wireframes, 21-screen-spec (subject to the STEP 7 human checkpoint)
  - S4-g4 technical: 30-functional-spec, 31-erd, 32-api-spec, 33-policy (reference 00, mutually consistent)
  - S4-g5 execution: 40-backlog, 41-qa-testcases
  - S4-g0 image assets (when possible, infographic-first tier ①): if image-generation capability exists, produce persona webtoons·ecosystem illustrations·concepts etc. into `docs/assets/` and reference them from the relevant md/HTML. If no capability, fall back to inline SVG/ASCII (skippable).
  - S4-g6 HTML output (STEP 1-b choice; md always preserved, after g1~g5 PASS + STEP 7 clearance): **both**[default] = each `.md`→readable `NN-name.html` (ASCII preserved + inline SVG infographics + generated images) + hub `index.html` (`references/html-doc-template.md`, reflecting INDEX.md) + diagram synthesis `overview.html` (`html-visual-template.md`) / **dashboard only** = `index.html` diagram synthesis (`html-visual-template.md`) / **per-document HTML only** = overview omitted / **none** = skip. No new content generation at all (only conversion·visualization of existing deliverables), 0 dependence on external renderers (Mermaid not recommended).
  - S4-g7 guide: `INDEX.md` — generated **last, after all documents (g1~g6) are complete**. 3-tier importance grouping of all deliverables (T1 essential / T2 conditional / T3 supporting) + work order (dependency-based) + per-role bundles + minimal starting set (Lean). Follows the "INDEX.md generation rules" in `references/mode-templates.md`. Judge the actual tiers per domain (e.g., for a payment type, 33·00 as T1). No new content generation — classification·sorting only.
  - **Order enforcement**: g1 (especially the 00-business-model money rules) must be locked first so g4 (ERD·API·policy doc) can stack consistently on top of it. Money rules are defined in only one place (00). Always last in the order g6 HTML → g7 INDEX.

### S4 — planning document integration writing → `service-plan.md` (final deliverable; Mode 5 is the per-group multiple documents above)

- **Purpose**: integrate the S1–S3 research into the selected mode structure (`mode-templates.md`). **No new research, integration + scope/priority/metrics finalization only.** (Mode 5 expands into multiple documents under `docs/`.)
- **Feeds**: C3, §8 gates G-a/G-d/G-e/G-g (and the placeholder slot for all of G-f).
- **Observable checks / Definition of Done**:
  - All sections of the selected mode are filled with real content (0 placeholders — G-f).
  - Every core feature is mapped to a specific problem in the problem definition (feature↔problem tracing table — G-a).
  - MVP / Out-of-scope are explicitly separated (G-e), and priorities are justified.
  - Every success metric has a measurable number + measurement method (tool/period/criterion) attached (G-d).
  - The differentiation section explicitly compares against at least 1 real competitor (G-c, leveraging the S1 basis).
  - Each document/section that has structure embeds a concept-fit visualization (diagram·infographic) (G-g — 0 text walls, 0 empty diagrams,
    0 dependence on external renderers such as Mermaid; generated images when possible).
  - Cross-section consistency is cross-checked.
- **Evaluator pass**: this sprint pass + **in the STEP 5 final integration pass, all 4 axes + 8 probes + all 7 §8 gates**.

## Sprint loop (common to each sprint)

```
1) Generator: propose sprint_contract.md (deliverable + observable checks + output file/format, mode-adapted)
2) Evaluator: approve or amend the contract (strengthen if the checks are weaker than the spec/playbook bars) — no building before approval
3) Generator: write the sprint deliverable → self-verify (all contract checks) → generator_report.md → READY_FOR_QA
4) Evaluator: evaluate the deliverable against the contract checks + sprint-relevant probes/gates → critique.md → PASS/FAIL
5) PASS → next sprint / FAIL & round<cap → retry the same sprint (Strategic Decision) / FAIL & round=cap → transparently report unresolved issues + user judgment
```

## Iteration cap (per sprint)

- **5–15 rounds per sprint, default start ~8** (a range, not a single value).
- Picking the low end (5) may miss a late-round breakthrough — "iteration 10 leap" warning (SKILL.md). The cap applies per sprint; the whole run = the sum of the 4 sprints.

## File flow (Full file set)

`spec.md`·`sprint-playbook.md` (Planner→) / `sprint_contract.md` (Generator⇄Evaluator, per sprint) / `research-s1.md`·`research-s2.md`·`research-s3.md` (sprint deliverables, read by subsequent sprints) / `service-plan.md` (S4 final) / `generator_report.md`·`critique.md` (per sprint + final) / `handoff.md` (under context pressure).
