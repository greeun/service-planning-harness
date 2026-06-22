# Rubric — service-planning-harness

> The evaluation rubric. Scores a service planning (서비스 기획) deliverable (service-planning document / 서비스 기획서) on 4 axes.
> The Evaluator uses this rubric in the per-sprint pass (only the relevant axes) and the final integrated pass (all 4 axes).
> For scoring calibration (the few-shot 1/3/5 anchors) see `evaluator-calibration.md`.

## Design principle — "Weight What Claude Lacks"

By default Claude fills structure, skeleton, and technical correctness well. What is weak without pressure is **originality, specificity, and domain taste**. So those weak axes get 2× weight. The axes where Claude fills the section skeleton well by default are left at 1×, but §8 binary gates block surface satisfaction (the section exists but the content is empty).

Style Magnet Warning: write criteria as **quality**, not as a **reference name**. Brand/name drops like "like Toss", "north-star-metric level", "McKinsey-grade" converge the Generator onto one direction, so they are banned. State the quality bar in plain words (e.g., "compared explicitly against at least 1 real competitor").

## The 4 evaluation criteria (C1–C4)

| # | Criterion (Korean) | Weight | What is measured | Justification for Claude's default weakness |
|---|--------------------|--------|------------------|---------------------------------------------|
| **C1** | **problem-definition clarity** (문제정의 명확성) | **1×** | Is whose / which / why problem defined sharply? Is the target persona concrete down to situation and pain points? | Claude fills a structured problem-definition section well by default (adequate by default) → 1×. But the target-specificity gate (G-b) prevents passing with abstract demographics only. |
| **C2** | **differentiation/originality** (차별화·독창성) | **2×** | Is there an explicit differentiator vs competitors/alternatives? Is it explicitly compared against at least 1 real competitor? | Without pressure Claude drifts to ungrounded generalities like "more convenient/faster than existing options" (table-stakes). An originality/specificity weak axis → 2× confirmed. |
| **C3** | **feasibility (MVP scope)** (실행가능성(MVP범위)) | **2×** | Is the scope realistic with clear priorities, and are MVP/non-MVP explicitly separated and immediately startable? | Without pressure Claude drifts to "let's build everything" over-scoping or vague scope. An actionability/specificity weak axis → 2× confirmed. |
| **C4** | **success-metric measurability** (성공지표 측정가능성) | **1×** | Does every success metric include a measurable number + measurement method? | Claude generates the KPI section itself by default (structure adequate) → 1×. But the measurability gate (G-d) FAILs non-measurable metrics like "improve satisfaction". |

**2× weights confirmed**: the 2× axes = **C2 differentiation/originality + C3 feasibility (MVP scope)**. C1/C4 are left at 1× because Claude fills the section skeleton by default, but §8's binary gates (G-a~G-g) block surface satisfaction.

## Score meaning (1–5)

| Score | Meaning |
|-------|---------|
| 5 | Exceeds expectations — would impress a demanding senior PM / planning lead |
| 4 | Meets expectations — solid practical quality |
| 3 | Acceptable but noticeably weak — needs reinforcement |
| 2 | Below expectations — meaningful gaps |
| 1 | Unacceptable — fundamental problems |

## Verdict Logic

```
All criteria ≥ 4 AND adversarial probes clean AND all §8 gates pass → PASS
Any 2×-weighted criterion (C2/C3) < 4                              → FAIL  (this is the core rule)
Any 1×-weighted criterion (C1/C4) < 3                              → FAIL
Any Definition-of-Done item or §8 gate unverified/failed          → FAIL
```

## Calibration Checkpoint

After scoring, if every axis is ≥4, apply the following second-pass lenses just before passing and add findings to the critique:

1. **Domain-expert lens**: "What would a demanding senior PM / planning lead catch?"
2. **Competitor lens**: "Where would a competitor point out this plan as a weakness?"
3. **User lens**: "Would the target user actually feel this service is worth using?"
4. **Infographic lens**: "Is content that has structure (hierarchy / flow / relationship / timeline / comparison / state transition) conveyed as a diagram/infographic, or is it a text wall?" — structure that would be conveyed better by a diagram but is left as prose/table only docks C1 (clarity) and C3 (readability/actionability). The presence and fit of visualization itself is separately enforced by the §8 gate **G-g (infographic fit, binary)** (empty diagram / external-renderer dependence such as Mermaid = FAIL).

In the per-sprint pass, score only the axes tied to that sprint (S1→C2, S2→C1, S3→part of C1 + wireframe gate, S4/final→all 4 axes). For the mapping details see `evaluator-prompt.md` and `sprint-playbook.md`.
