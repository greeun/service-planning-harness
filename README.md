# service-planning-harness

> 🇰🇷 [한국어 README](./README.ko.md)

A Claude Code skill that turns a one-to-four-sentence service idea into a **development-ready service-planning document** — or a full **16-deliverable planning package with a visual HTML dashboard** — using a three-agent **GAN-style harness** (Planner → Generator → Evaluator) at the **Full tier**.

Built on the Planner–Generator–Evaluator pattern from Anthropic's *Harness Design for Long-Running Application Development*. Generation and evaluation run as **separate `Agent` dispatches** so that self-evaluation bias can't silently wave through mediocre work.

---

## Why

LLMs left to "just write a plan" tend to produce confident, structurally complete, but shallow output: differentiation collapses into "more convenient / faster," scope balloons to "build everything," and success metrics read "improve satisfaction." This skill forces the opposite:

- **Research is gated before synthesis.** Four research sprints (market → problem → UX → integration) each pass an adversarial Evaluator before the plan is built on top of them.
- **Claude's weak axes get 2× weight.** Differentiation and actionability — the two things Claude underdelivers without pressure — dominate the rubric.
- **Every claim is checkable.** Six binary gates (feature↔problem traceability, persona specificity, named-competitor comparison, metric measurability, MVP/out-of-scope split, zero placeholders) must all pass.

---

## How it works

```
[Main session]  ── intake: which mode? + 1–4 sentence idea
      │
      ▼
STEP 3  Planner  ─────►  spec.md + sprint-playbook.md   (mode-specific structure)
      │
      ▼
STEP 4  4 research sprints (each: contract → build → evaluate, cap 5–15)
      │   S1 market·competition → research-s1.md
      │   S2 problem·target·persona → research-s2.md
      │   S3 UX/UI flows·screens → research-s3.md
      │   S4 integration → service-plan.md  (Mode 5: groups g1–g6 → package/)
      ▼
STEP 5  Final integrated Evaluator pass  (4-axis rubric + 7 probes + 6 gates)
      │
      ▼
STEP 7  Conditional wireframe human-checkpoint  (fires only if a wireframe is present)
      ▼
STEP 7.5  Visual HTML dashboard render  (index.html)
      ▼
STEP 7.6  Importance-grouped index  (INDEX.md — tiers · work order · roles · lean set)
      ▼
STEP 8  Deliver
```

The Generator and Evaluator never see each other's reasoning — they communicate only through files (`spec.md`, `sprint_contract.md`, `research-s*.md`, `critique.md`, `handoff.md`).

---

## Output modes

Activation always asks which mode to produce first (STEP 1, mandatory gate):

| Mode | Name | Produces |
|------|------|----------|
| 1 | **Lean MVP plan** *(default)* | Problem → target → core features → user flow → MVP scope → metrics. Buildable in ~4 weeks. Light S3, no wireframes. |
| 2 | **Full PRD** | + functional spec / screen definitions / edge cases / non-functional requirements. |
| 3 | **Business + product** | + market / competition / revenue-model analysis. Expanded S1. |
| 4 | **Screen & feature spec** | User flows / per-screen features / data structure. Heaviest S3 + wireframes. |
| 5 | **Full planning package** | ~16 deliverables across 5 stages **+ a visual HTML dashboard** (`index.html`) **+ an importance-grouped index** (`INDEX.md`). |

### Mode 5 — the full package

```
package/
├─ Discovery   00-business-model · 01-service-plan · 02-market-competition · 03-personas
├─ Definition  10-prd · 11-user-stories · 12-ia · 13-user-flows
├─ Design      20-wireframes · 21-screen-spec
├─ Technical   30-functional-spec · 31-erd · 32-api-spec · 33-policy
├─ Execution   40-backlog · 41-qa-testcases
├─ Final read  index.html   (mind map · flowcharts · architecture · ERD · charts · infographics · webtoon panels)
└─ Guide       INDEX.md     (importance tiers · work order · per-role bundles · minimal start set)
```

A **single-source rule** is enforced: money rules (pricing, take-rate, settlement triggers) are defined only in `00-business-model.md`; ERD, API, and policy docs reference it and never redefine.

---

## Evaluation rubric

| # | Criterion | Weight | Why |
|---|-----------|--------|-----|
| C1 | Problem clarity | 1× | Claude fills problem sections adequately by default; backstopped by a target-specificity gate. |
| C2 | **Differentiation / originality** | **2×** | Claude's weak-by-default axis — drifts to table-stakes generalities under no pressure. |
| C3 | **Feasibility (MVP scope)** | **2×** | Claude over-scopes without pressure; needs explicit MVP / out-of-scope split. |
| C4 | Success-metric measurability | 1× | Section structure is adequate by default; backstopped by a measurability gate. |

**Verdict:** all criteria ≥ 4, every adversarial probe clean, and all six §8 gates pass → **PASS**. Any 2×-weighted criterion < 4 → **FAIL**. Scoring is anchored to concrete 1/3/5 few-shot examples in `references/evaluator-calibration.md`.

---

## Visual HTML dashboard

The final deliverable renders every artifact into a **self-contained, diagram-rich HTML dashboard** so the whole plan is graspable at a glance — mind map, value-loop and money-flow infographics, competitive matrix, webtoon-style persona panels, user-flow charts, IA tree, system architecture, ERD, metric gauges, scope infographic, and a backlog Gantt. Diagrams use Mermaid; infographics are inline SVG/CSS (work offline). See `references/html-visual-template.md`.

---

## Importance grouping (INDEX.md)

Not every project needs all 16 docs. After the package is written, Mode 5 emits `INDEX.md` — a guide that classifies the deliverables instead of dumping them:

- **Importance tiers** — T1 *must-have* (can't start dev without them) / T2 *conditional* (required by domain — e.g. `33-policy`, `00-business-model` for payment/regulated services) / T3 *supporting* (alignment, pitch, onboarding). Tiers are judged per-service, not fixed.
- **Work order** — dependency-based reading/build sequence (money rules first → why/who → what → screens → data/API → execution → visual summary).
- **Per-role bundles** — developer / QA / designer / PO / leadership each get the subset they actually read.
- **Minimal start set** — the lean ~6 docs (`10-prd` · `13-user-flows` · `21-screen-spec` · `31-erd` · `32-api-spec` · `40-backlog`) sufficient to begin building; the full 16 are for kickoff alignment, client handoff, or regulated domains.

This keeps the package from becoming documentation theater — it tells the reader *what to read first, and who reads what*.

---

## Usage

Trigger phrases:

- **Korean:** "서비스 기획", "기획서 작성", "서비스 기획서", "PRD 작성", "MVP 기획", "제품 기획", "서비스 기획 스킬", "화면 명세 작성", "전체 기획 산출물", "기획 패키지"
- **English:** "service planning", "write PRD", "product spec", "MVP plan", "full planning package"

The skill does **not** trigger on code debugging or content writing — it only produces planning deliverables.

---

## Directory structure

```
service-planning-harness/
├─ SKILL.md                          # orchestrator: 8-step flow, mode gate, sprint loop, tuning loop
└─ references/
   ├─ planner-prompt.md              # Planner: mode selection + spec/sprint-playbook
   ├─ generator-prompt.md            # Generator: per-sprint contract → build → self-verify
   ├─ evaluator-prompt.md            # Evaluator: per-sprint + final pass, 7 adversarial probes
   ├─ evaluator-calibration.md       # few-shot 1/3/5 anchors for C1–C4
   ├─ rubric.md                      # 4 criteria, 2× weights, verdict logic
   ├─ sprint-playbook.md             # 4 research sprints + Mode 5 group expansion (g1–g6)
   ├─ mode-templates.md              # 5 output-mode document skeletons
   └─ html-visual-template.md        # visual HTML dashboard render rules + skeleton
```

---

## Tier choice (Full vs Simplified)

This skill runs at the **Full tier** (sprint contracts + per-sprint Evaluator passes) **by deliberate choice**, even on strong models where context anxiety is eliminated. The reason is orthogonal to context management: service planning needs **research-phase decomposition + per-phase quality gating** — weak market research or a fuzzy problem definition must be caught *before* a plan is synthesized on top of it.

| Model | Context anxiety | Default tier | This skill |
|-------|----------------|--------------|------------|
| Sonnet 4.5 | Strong | Full | Full |
| Opus 4.5 | Largely gone | Simplified | Full (research gating) |
| Opus 4.6 | Gone | Simplified / Single-session | Full (research gating) |
| Opus 4.8 | Gone (1M ctx) | Simplified | **Full (deliberate)** |

On a future model upgrade, the stress-test is: *can the model self-organize the four research phases and self-gate their quality without explicit sprint contracts?* If yes, drop sprints one at a time — never all at once.

---

## Example run

Tested end-to-end on **character-market** (a two-sided AI-character + creator-upload marketplace), Mode 1 → Mode 5. All four research sprints and the final integrated pass returned **PASS** (C1 5 · C2 5 · C3 5 · C4 4; all six gates pass). The full package (16 deliverables + visual dashboard) lives under the project's `planning/package/`.

---

## Design principles

1. **GAN role separation** — Generator and Evaluator are separate `Agent` dispatches; a single session grading its own work confidently praises mediocrity.
2. **File-based handoffs only** — agents never share reasoning, only files.
3. **Sprint contracts before work** — the Evaluator approves each sprint's observable checks before the Generator builds.
4. **Reset, don't compact** — context pressure recovers via `handoff.md` + a fresh session, never compaction.
5. **Rubric with evidence** — every score cites an exact quote and location; "looks fine" never passes.
6. **Every component encodes an assumption** — on model upgrade, stress-test each and drop what is no longer load-bearing.

> Anthropic, *Building Effective Agents*: "find the simplest solution possible, and only increase complexity when needed."

---

## Credits

Generated and audited with [`skill-wizard-harness`](../skill-wizard-harness), itself an application of the Planner–Generator–Evaluator meta-pattern.
