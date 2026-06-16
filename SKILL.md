---
name: service-planning-harness
description: 짧은 서비스 아이디어나 요구사항을 받아 **개발 착수 가능한 서비스 기획서(또는 전체 기획 산출물 패키지)**를 산출하는 **3-에이전트 GAN 하니스 스킬**(Planner→Generator→Evaluator, Full tier). 활성화 시 5가지 산출물 모드(Lean MVP 기획서 / 정식 PRD / 비즈니스+기획 통합 / 화면·기능 명세 중심 / 풀 기획 패키지) 중 하나를 먼저 선택하게 한 뒤, 4개 리서치 스프린트(S1 시장·경쟁 리서치 → S2 문제·타겟 → S3 UX/UI 플로우 → S4 기획서 통합작성)를 스프린트별 계약·품질게이트로 진행하며 선택 모드에 맞춰 문서 구조를 조정해 생성하고, 별도 Evaluator가 스프린트별 + 최종 4축 루브릭으로 적대적 검증한다. 풀 기획 패키지(Mode 5)는 5단계 ~16종 산출물(기획서·PRD·IA·유저플로우·화면설계서·기능명세·ERD·API명세·정책서·백로그·QA 등)을 한 번에 생성한다. 다음과 같이 요청할 때 사용: "서비스 기획", "기획서 작성", "서비스 기획서", "PRD 작성", "MVP 기획", "제품 기획", "서비스 기획 스킬", "화면 명세 작성", "전체 기획 산출물", "기획 패키지"; English: "service planning", "write PRD", "product spec", "MVP plan", "full planning package". (트리거 14개: 한국어 10 + 영어 5.)
---

# Service Planning Harness (서비스 기획 하니스)

짧은 서비스 아이디어(1–4문장)를 받아, 엔지니어링 팀이 바로 착수할 수 있는 **서비스 기획서(service-planning document)**를 산출하는 GAN 스타일 3-에이전트 하니스다. Planner → Generator → Evaluator를 각각 별도의 `Agent` 콜로 디스패치한다(GAN anti-omission-bias). **Full tier**: 4개 리서치 스프린트를 스프린트별 계약 협상 + 품질 게이트로 진행하고, 스프린트별 + 최종 통합 Evaluator 패스를 돌린다.

## 핵심 설계 의도 (왜 Full tier인가)

서비스 기획은 **리서치-페이즈 분해 + 페이즈별 품질 게이팅**을 요구한다. 약한 시장조사/문제정의 위에 기획서를 쌓으면 차별화(C2)·실행가능성(C3)이 구조적으로 무너진다. 그래서 S1–S3 리서치를 Evaluator가 게이트로 통과시킨 **뒤에만** S4가 기획서를 합성한다. 이 게이팅은 컨텍스트 완화가 목적이 아니라 직교적 품질 메커니즘이다. (모델 가이던스 절 참조 — Opus 4.8은 컨텍스트 불안이 사실상 없지만 Full을 의도적으로 선택했다.)

## 참조 파일 (references/)

| 파일 | 역할 |
|------|------|
| `references/planner-prompt.md` | Planner 프롬프트 — 4-모드 선택 처리 + spec.md/sprint-playbook.md 작성 |
| `references/generator-prompt.md` | Generator 프롬프트 — 스프린트별 계약 협상 + 산출 + self-verify |
| `references/evaluator-prompt.md` | Evaluator 프롬프트 — 스프린트별 + 최종 패스, 7 probe, 판정 로직 |
| `references/rubric.md` | 4 기준(C1–C4), 가중(C2/C3 2×), 정당화, 판정 로직 |
| `references/evaluator-calibration.md` | few-shot 1/3/5 앵커 (C1–C4) — 채점 보정 |
| `references/sprint-playbook.md` | 4-스프린트 계획, deliverable, 모드 적응 범위, 관측가능 체크 |
| `references/mode-templates.md` | 5 출력 모드의 문서 구조 스켈레톤 (Mode 5 풀패키지 16종 포함) |
| `references/html-visual-template.md` | 최종 산출물을 도식 시각화 중심 자체완결 HTML 대시보드로 렌더(마인드맵·다이어그램·순서도·아키텍처·차트·인포그래픽·웹툰). STEP 8 직전 렌더 |

## 4개 산출물 모드

| 모드 | 이름 | 요지 |
|------|------|------|
| 1 | Lean MVP 기획서 [기본/권장] | 4주 착수 가능한 최소 기획서. S3 라이트, 와이어프레임 없음. |
| 2 | 정식 PRD | + 기능명세 / 화면정의 / 엣지케이스 / 비기능요구. |
| 3 | 비즈니스+기획 통합 | + 시장 / 경쟁 / 수익모델 분석. S1 확장. |
| 4 | 화면·기능 명세 중심 | 유저플로우 / 화면별 기능 / 데이터 구조 중심. S3 최중량 + 와이어프레임. |
| 5 | 풀 기획 패키지 | 5단계 ~16종 산출물 세트(발견·정의·설계·기술·실행) + **최종 비주얼 HTML 대시보드(`index.html`)**. S1~S3 리서치 후 S4가 다중 문서 생성(g1~g6)으로 확장. 단일 service-plan.md가 아니라 `package/` 디렉터리에 번호 매긴 문서 묶음 + 도식 시각화 HTML 산출. |

상세 섹션 구조는 `references/mode-templates.md`. Mode 5의 산출물 목록·문서 골격도 거기 정의.

## 4개 리서치 스프린트 (고정, 범위 가변)

| 스프린트 | 이름 | deliverable | feeds | 모드 적응 |
|----------|------|-------------|-------|-----------|
| S1 | 자료조사/시장·경쟁 | `research-s1.md` | C2, G-c | Mode 3 확장(시장/수익모델); 그 외 최소 1개 경쟁 명시 |
| S2 | 문제·타겟·페르소나 | `research-s2.md` | C1, G-a/G-b | Mode 1 린(1 페르소나); 그 외 복수 가능 |
| S3 | UX/UI 플로우·화면 | `research-s3.md` | P-1, G-a | Mode 1 라이트(와이어프레임 없음); Mode 4 최중량 |
| S4 | 기획서 통합작성 | `service-plan.md` | C3, G-a/G-d/G-e | S1–S3을 선택 모드 구조로 통합 (신규 리서치 없음) |

4개 스프린트는 **항상** 실행되고, 각 스프린트의 범위만 모드에 따라 적응한다. 상세는 `references/sprint-playbook.md`.

---

## Activation Flow (오케스트레이터)

8-step 흐름. STEP 1 = 출력 모드 선택(필수 게이트). Full tier 4-스프린트 루프(STEP 4)에 스프린트별 계약 협상 + 평가.

### STEP 1 — 모드 선택 (mandatory gate)
활성화 즉시 사용자에게 5개 모드를 제시하고 선택을 받는다 — (1) Lean MVP 기획서 [기본/권장], (2) 정식 PRD, (3) 비즈니스+기획 통합, (4) 화면·기능 명세 중심, (5) 풀 기획 패키지(5단계 ~16종 산출물). 사용자가 미응답/모호하면 기본값 (1) Lean MVP를 **제안**하되, 확정 응답을 받기 전엔 생성하지 않는다. 선택된 모드를 `spec.md` / `sprint-playbook.md` 입력으로 동결한다. **Mode 5는 단일 문서가 아니라 다중 산출물**이므로, STEP 4 S4가 deliverable-그룹별로 확장되고 STEP 7 와이어프레임 게이트가 거의 항상 발동한다(화면설계서 포함).

### STEP 2 — 입력 수집
1–4문장 서비스 아이디어/요구사항을 받는다. 너무 모호하면 핵심 1~2개만 되묻고 진행한다(과도한 질문 금지). 그래도 모호하면 Planner가 가장 날카로운 가설 1개를 세워 spec/playbook에 가정으로 표기한다.

### STEP 3 — Planner 디스패치
`Agent` 콜로 `references/planner-prompt.md`를 Planner로 1회 실행. 동결된 모드의 섹션 구조로 `spec.md`를 작성하고, **4-스프린트 계획 + 각 스프린트의 모드 적응 범위를 `sprint-playbook.md`에 작성**한다. 반환: `SPEC_READY: spec.md` + `PLAYBOOK_READY: sprint-playbook.md`.

### STEP 4 — 4-스프린트 루프 (S1→S2→S3→S4, 각 스프린트 cap 5–15)
각 스프린트마다 순서대로:

- **4a — 계약 협상**: Generator(`Agent` 콜, `references/generator-prompt.md`)가 해당 스프린트의 `sprint_contract.md`(범위 + 관측가능 체크 + 출력 파일/포맷)를 제안한다. Evaluator(`Agent` 콜, `references/evaluator-prompt.md`, Mode A1)를 디스패치해 계약을 **승인/수정**한다. 체크가 약하면 Evaluator가 강화한다. **승인 전엔 빌드 금지(safety gate).**
- **4b — 스프린트 빌드**: Generator를 `Agent` 콜로 디스패치. 해당 스프린트 산출물(`research-s1.md`/`-s2.md`/`-s3.md`, 또는 S4의 `service-plan.md`)과 `generator_report.md`를 산출하고 핸드오프 전 self-verify. 반환: `READY_FOR_QA` 또는 `HANDOFF_NEEDED`(→ 4d) 또는 `DEADLOCK`(→ STEP 8).
- **4c — 스프린트 평가**: Evaluator를 `Agent` 콜(Mode A2)로 디스패치해 그 스프린트의 관측가능 체크 + 스프린트 관련 probe/게이트로 `critique.md`를 산출. `PASS`면 다음 스프린트로; `FAIL`이고 라운드 < cap이면 같은 스프린트 재시도(Generator의 Strategic Decision: REFINE/PIVOT 적용); `FAIL`이고 라운드 = cap이면 미해결 이슈를 투명 기록하고 사용자 판단을 받아 다음 스프린트 진행 여부를 결정.
- **4d — 컨텍스트 핸드오프**: `HANDOFF_NEEDED`면 fresh Generator 세션을 `handoff.md`만 읽혀 해당 스프린트를 재개한다. **컴팩션 금지** — 컴팩션은 불안 상태를 보존하므로 회복 경로로 금지(reset ≠ compaction).

### STEP 5 — 최종 통합 Evaluator 패스
S4 완료 후, `Agent` 콜로 Evaluator(Mode B)를 디스패치해 통합 `service-plan.md`에 대해 **4축 루브릭 점수 + 7개 적대적 probe + §8 6게이트 전부**를 실행해 `critique.md` 산출. 반환: `CRITIQUE_READY: critique.md`.

### STEP 6 — 판정 분기
- `PASS` (All ≥4, 2×(C2/C3) ≥4, probes clean, §8 전부 통과) → STEP 7.
- `FAIL` → 결손이 특정 스프린트 산출물에 귀속되면 해당 스프린트로 복귀(STEP 4, cap 내), 통합 단계 결손이면 S4 재시도. 전체 cap 도달 시 현재 최선본 + 미해결 blocking issue를 투명 보고.

### STEP 7 — 조건부 와이어프레임 인간 체크포인트 (P-1, S3 직후가 가장 관련 큼)
`research-s3.md` / `service-plan.md`에 **와이어프레임/화면 목업 블록**이 포함됐는지 검사한다.
- **포함 안 됨 (모드 1/3 일반)** → 체크포인트 **스킵**, 완전 자동, STEP 8로 진행.
- **포함됨 (모드 2/4 가능)** → **사용자에게 와이어프레임 검토·승인을 요청**하고, 승인 전엔 최종 PASS를 확정하지 않는다. 텍스트 부분은 정상 PASS, 시각 부분은 `HUMAN_CHECKPOINT_REQUIRED`로 표시해 사용자에게 노출한다.

> 이 게이트의 근거: 텍스트 산출물은 LLM이 모든 단어를 읽고 판단 가능하나(sensory limits = none), UI 와이어프레임/목업의 시각적 craft는 LLM 미적 판단의 신뢰 범위를 넘는다. 따라서 그 경우에만 인간 체크포인트를 삽입한다. "Evaluator가 알아서 처리"는 허용되지 않는다 — 시각 승인은 사람이 한다.

### STEP 7.5 — 비주얼 HTML 렌더 (최종 읽기 문서)
PASS된 최종 산출물을 `references/html-visual-template.md`에 따라 **도식 시각화 중심 자체완결 HTML 대시보드**로 렌더한다. Mode 1~4 → `<service>-plan.html`, Mode 5 → `package/index.html`. 시각화 카탈로그(마인드맵·가치순환·자금흐름·경쟁매트릭스·페르소나 웹툰·유저플로우·IA트리·아키텍처·ERD·지표차트·범위 인포그래픽·간트 등)에서 해당 모드에 맞는 것을 **실제 데이터로** 채운다. HTML은 새 내용 생성 금지 — 기존 산출물의 구조·수치·관계만 시각화(돈 규칙은 단일출처 인용). 와이어프레임/시각 craft가 포함되면 STEP 7 인간 체크포인트 통과 후 렌더.

### STEP 8 — 산출/종료
최종 `service-plan.md`(Mode 5는 `package/` 16종 + `index.html`)를 사용자에게 전달한다. `DEADLOCK` 발생 시 양측 입장을 요약해 사용자 판단을 요청한다.

### 사용자 확인 게이트 요약
(a) STEP 1 모드 확정 전 생성 금지; (b) STEP 4a 계약 미승인 시 빌드 금지; (c) STEP 7 와이어프레임 포함 시 사용자 승인 전 PASS 확정 금지; (d) cap 도달 시 미해결 이슈 투명 보고.

### Safety gates (도메인)
모드 확정 게이트(STEP 1), 스프린트별 계약 승인 게이트(STEP 4a), 조건부 와이어프레임 인간 체크포인트(STEP 7), §8 이진 검증 게이트(STEP 4c/STEP 5).

---

## 반복 캡 & 반복 지혜 (Iteration Wisdom)

- **iteration cap = 5–15 per sprint, 기본 시작 ~8** — 이것은 **범위이지 단일값이 아니다.** 캡은 각 스프린트(S1–S4)에 개별 적용되고, 전체 런은 4개 스프린트의 합이다.
- **저점(5) 경고**: 스프린트를 너무 일찍(예: 3회) 끊으면 **후반 라운드의 돌파**를 놓칠 수 있다. 한 사례에서 가장 좋은 결과가 반복 10회차에서 나왔다 — "Dutch Art Museum leap at iteration 10". 캡 저점을 고르면 이런 late-iteration breakthrough를 잘라낼 위험이 있다.
- **벽시계 관용**: 인위적으로 서두르지 않는다. 좋은 기획서는 시간이 걸린다 — 진행이 느리다는 이유만으로 라운드를 줄이지 않는다.
- **중간 반복이 최종보다 나을 수 있다**: Evaluator는 `critique.md`에 **Iteration Quality Note**를 남겨, 이전 라운드가 더 나았던 강점이 있으면 명시한다. 그 강점을 잃지 않도록 다음 라운드에서 보존한다.

---

## 원칙 (Principles)

- **reset ≠ compaction**: 컨텍스트 압박 시 회복 경로는 `handoff.md` + fresh 세션이지 컴팩션이 아니다. 컴팩션은 불안 상태를 보존하므로 회복 경로로 **금지**한다. Opus 4.8에서는 컨텍스트 불안이 드물지만, 발생 시 이 규칙을 따른다.
- **모든 컴포넌트는 가정을 인코딩한다**: 이 하니스의 각 컴포넌트는 "모델이 혼자 못 하는 무언가"에 대한 가정을 담는다. 여기서 스프린트 구조의 가정은 "합성 전에 리서치 품질을 게이트해야 한다"이다. 모델 업그레이드 시 가정을 **하나씩** 검증하고 — 한 컴포넌트를 한 번에 하나씩 제거해 측정한다(radical 한꺼번에 제거는 실패했고, methodical one-at-a-time이 성공했다).
- **GAN 분리**: Generator와 Evaluator를 별도 `Agent`로 분리하는 이유는 self-evaluation bias(LLM이 평범한 결과를 자신 있게 칭찬하는 경향)를 구조적으로 상쇄하기 위함이다.
- **단순성 우선** (Anthropic, "Building Effective Agents"): "find the simplest solution possible, and only increase complexity when needed." 이 스킬이 Full을 택한 것은 단순성 원칙 위반이 아니라 — 리서치 게이팅이라는 직교적 필요 때문이며, 그 필요가 사라지면 단순화한다.

---

## V1 vs V2 guidance (모델별)

이 스킬은 **Full tier(V1 계열)**다. 모델 클래스별 권장 tier:

| Model class | Context anxiety | Recommended tier | Notes |
|-------------|----------------|-----------------|-------|
| **Sonnet 4.5** | Strong — wraps up prematurely | Full (V1) | Small sprints, aggressive Evaluator, firm context resets |
| **Opus 4.5** | Largely eliminated | Simplified (V2) | Multi-hour coherent sessions; sprint decomposition droppable |
| **Opus 4.6** | Eliminated; improved planning, long-context, debugging | Simplified or Single-session | 2+ hour builds sustainable; re-examine every component, drop what's not load-bearing |
| **Opus 4.8** | Eliminated (1M context) | Simplified by default — BUT this skill chooses Full deliberately | Full chosen NOT for context relief but for research-phase decomposition + per-sprint quality gating (S1–S3 research gated before S4 synthesis). Stress-test on upgrade (G-1/G-2): can the model self-organize the 4 research phases + self-gate quality without sprint contracts? If yes, drop sprints one at a time. |

General rule: every harness component encodes an assumption about what the model can't do alone. On model upgrade, stress-test each assumption individually — remove one component at a time and measure. Cite Anthropic, "Building Effective Agents" — "find the simplest solution possible, and only increase complexity when needed".

### Opus 4.8: Full vs Simplified 정당화 (G-1/G-2)
Opus 4.8(1M 컨텍스트)은 컨텍스트 불안이 사실상 제거됐다 — 컨텍스트 관점에서는 스프린트 구조가 redundant하므로 article은 **Simplified**를 기본으로 권한다. 그러나 여기서는 **Full을 의도적으로 선택**했다. 이유: 이 스프린트 구조는 컨텍스트 완화 장치가 아니라 **직교적(orthogonal) 메커니즘** — 리서치-페이즈 분해 + 페이즈별 품질 게이팅(S1–S3 리서치를 Evaluator가 게이트로 통과시킨 뒤에만 S4가 그 위에 기획서를 합성)이기 때문이다. 강한 모델도 이 게이팅의 이득을 잃지 않는다(가정: "합성 전에 리서치 품질을 게이트해야 한다" — 모델 강함이 이를 무효화하지 않음).

**업그레이드 스트레스 테스트 (G-1/G-2, 하나씩)**: "모델이 명시 sprint contract 없이 4개 리서치 페이즈를 스스로 조직하고 품질을 self-gate할 수 있는가?" YES면 스프린트 구조를 **한 스프린트/계약씩** 제거하며 재측정한다 — 전부 한꺼번에 제거하지 않는다(radical 제거는 실패, methodical one-at-a-time 성공). 하니스 공간은 줄어드는 게 아니라 이동한다 — 무엇이 load-bearing인지 매 업그레이드마다 재검토한다.

---

## Evaluator tuning workflow

untuned Evaluator는 너무 관대하다. 첫 런들은 draft로 취급하고, 다음 **운영 루프**로 보정한다(G-4/P-3). 한 문장 인정으로 끝내지 말고 (a)–(d)를 실제로 돌린다:

- **(a) Read critique logs** — 완료된 런들의 `critique.md`를 실제 산출물과 나란히 읽는다. 각 루브릭 점수에 대해 묻는다: "까다로운 시니어 PM / 기획 리드라면 같은 점수를 줬을까?"
- **(b) Identify divergence patterns** — 발산 패턴을 특정한다. 전형: 관대 채점(generic/shallow 산출을 적정으로 수용), 미매핑 기능 누락, 측정 불가 지표를 통과시킴, 추상 페르소나를 C1 합격, 표면 외형만 보고 substance 미확인.
- **(c) Update prompt or calibration with counter-examples** — `references/evaluator-prompt.md` 또는 `references/evaluator-calibration.md`에 그 실패 모드를 겨냥한 **구체 반례**를 추가한다(예: "측정 가능해 보이나 판별 기준이 빠진 지표는 5/5가 아니라 3/5"). few-shot 보정이 score drift를 줄인다.
- **(d) Rerun on the same input; confirm the catch** — 같은 입력으로 재실행해 Evaluator가 이전 누락을 이제 잡아내는지 확인한다. 잡으면 그 보정을 고정한다.

(a)–(d) 중 하나라도 건너뛰면 튜닝이 아니다. Evaluator 판정이 신중한 인간 전문가 패스와 상관하고, 제기하는 모든 blocking issue가 재현 가능해질 때까지 반복한다(여러 사이클 예상).

---

## §8 도메인 검증 게이트 (binary)

최종 PASS 전 6개 이진 게이트 전부 통과해야 한다(Generator가 S4 핸드오프 전 self-check, Evaluator가 STEP 4c/STEP 5에서 검증). 상세는 `references/sprint-playbook.md` / `references/evaluator-prompt.md`.

| Gate | Check | PASS / FAIL |
|------|-------|-------------|
| G-a 기능↔문제 추적 | 모든 핵심기능이 특정 문제에 매핑 | 모든 기능 ≥1 문제 연결 / 미매핑 기능 1개라도 |
| G-b 타겟 구체성 | 페르소나 ≥1명 상황·페인포인트 포함 | 상황·맥락·페인포인트 명시 / 추상 인구통계만 |
| G-c 차별화 명시 비교 | ≥1개 실제 경쟁과 명시 비교 | 경쟁자명+비교축 존재 / 미특정 or "더 좋다"식 |
| G-d 성공지표 측정성 | 모든 지표 측정 수치+방법 | 각 지표 수치·도구·기간 / 비측정 지표 1개라도 |
| G-e MVP 범위/비범위 분리 | MVP / out-of-scope 명시 구분 | 분리 존재 / 구분 없음 or 모호 |
| G-f 자리표시자 없음 | placeholder/TBD/빈 칸 0 | 스캔 결과 0개 / 1개라도 |

---

## 파일 핸드오프 (file-based communication, full set)

역할은 파일로만 소통한다. 활성 파일:
`spec.md`·`sprint-playbook.md` (Planner→) / `sprint_contract.md` (Generator⇄Evaluator, 스프린트별) / `research-s1.md`·`research-s2.md`·`research-s3.md` (스프린트 산출물, 후속 스프린트가 읽음) / `service-plan.md` (S4 최종 기획서) / `generator_report.md`·`critique.md` (스프린트별 + 최종) / `handoff.md` (Generator → fresh Generator, 컨텍스트 압박 시).

각 역할은 fresh `Agent` 콜로 디스패치돼 자신의 핸드오프 파일만 읽는다. Planner는 1회 실행, Generator와 Evaluator는 각 스프린트 내 + 4개 스프린트에 걸쳐 교대한다.

---

## 범위 밖 (out-of-scope)
코드 디버깅("이 React 컴포넌트 버그 고쳐줘")·콘텐츠 작성("블로그 글 써줘")은 서비스 기획서 산출이 아니므로 이 스킬을 활성화하지 않는다. description의 트리거(서비스 기획/PRD/MVP 기획/화면 명세 등)와 매칭되는 경우에만 활성화한다.
