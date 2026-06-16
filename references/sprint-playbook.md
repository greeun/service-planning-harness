# Sprint Playbook — service-planning-harness (Full tier)

> REQUIRED Full-tier 파일. 4개 고정 리서치 스프린트의 계획, 각 스프린트의 deliverable 파일, 모드 적응 범위, 관측가능 체크를 정의한다.
> Planner가 STEP 3에서 사용자 선택 모드를 반영해 이 플레이북의 인스턴스를 `sprint-playbook.md`로 출력한다(아래 구조 + 모드 적응 범위를 채워서). Generator는 각 스프린트 시작 시 이 플레이북을 읽고 `sprint_contract.md`를 제안하며, Evaluator는 이 플레이북의 관측가능 바에 비추어 계약을 승인/수정하고 스프린트 산출물을 평가한다.

## 왜 4 스프린트인가 (Full tier 정당화)

서비스 기획은 **리서치-페이즈 분해 + 페이즈별 품질 게이팅**을 요구한다. 약한 시장조사/문제정의 위에 기획서를 쌓으면 차별화(C2)·실행가능성(C3)이 구조적으로 무너진다. 따라서 S1–S3 리서치 산출물을 Evaluator가 게이트로 통과시킨 **뒤에만** S4가 그 위에서 기획서를 합성한다. 이 게이팅은 컨텍스트 완화 목적이 아니라 직교적(orthogonal) 품질 메커니즘이다 (모델 강함이 이 가정을 무효화하지 않음 — SKILL.md 모델 가이던스 참조).

## 4개 고정 스프린트

모든 모드에서 4개 스프린트가 **항상** 실행된다. 각 스프린트의 `sprint_contract.md` 범위가 선택 모드에 따라 적응한다.

### S1 — 자료조사 / 시장·경쟁 리서치 → `research-s1.md`

- **목적**: 시장 맥락 + 최소 1개 실제 경쟁자/대안 특정 + 벤치마크. 차별화(C2)의 근거가 된다.
- **Feeds**: C2, §8 게이트 G-c (차별화 명시 비교).
- **관측가능 체크 (계약에 포함될 바)**:
  - 최소 1개 **실제(이름 있는)** 경쟁자/대안이 특정됐다.
  - 각 경쟁자에 대해 비교축(무엇을 하고/안 하는지)이 기록됐다.
  - 시장/사용자 맥락이 기술됐다.
- **모드 적응 범위**:
  - Mode 3 (비즈니스+기획): 확장 — 시장규모(TAM/SAM/SOM 근거)·세분시장·수익모델 분석까지.
  - Modes 1/2/4: 최소 — "최소 1개 명시 경쟁 비교" + 맥락.
- **Evaluator 스프린트 패스**: C2 채점 + G-c 게이트 + 차별화 명시 비교 probe.

### S2 — 문제·타겟·페르소나 리서치 → `research-s2.md`

- **목적**: who-what-why 문제정의 + 구체 페르소나(상황·페인포인트). 문제정의 명확성(C1)의 근거.
- **Feeds**: C1, §8 게이트 G-a(추적의 출발점인 문제 목록), G-b (타겟 구체성).
- **관측가능 체크**:
  - 문제가 누구의/어떤/왜 형태로 날카롭게 정의됐다.
  - 최소 1명 페르소나가 상황·맥락·페인포인트(빈도/결과 포함)까지 구체적이다 — 추상 인구통계만이면 불합격.
- **모드 적응 범위**:
  - Mode 1 (Lean): 1 핵심 페르소나로 린하게.
  - Modes 2/3/4: 필요 시 복수 페르소나, 단 핵심 1명은 반드시 구체.
- **Evaluator 스프린트 패스**: C1 채점 + G-a(문제 목록 존재)/G-b 게이트 + 타겟 구체성 probe.

### S3 — UX/UI 플로우·화면 리서치 → `research-s3.md`

- **목적**: 유저플로우 + 화면별 기능 + (모드에 따라) 와이어프레임/화면 구조.
- **Feeds**: 와이어프레임 게이트(STEP 7 / P-1), G-a (기능↔문제 추적의 기능 후보).
- **관측가능 체크**:
  - 핵심 유저플로우가 정의됐다.
  - 화면/단계별 기능이 정리됐다.
  - (모드에 따라) 와이어프레임/화면 구조 블록 존재 여부.
- **모드 적응 범위**:
  - Mode 1 (Lean): 라이트 — 핵심 유저플로우만, **와이어프레임 없음**.
  - Mode 2 (PRD): + 화면정의 + 엣지케이스.
  - Mode 3 (비즈니스): 표준 플로우 + 화면.
  - Mode 4 (화면·기능 명세): **최중량** — 화면별 상세 기능 + 데이터 구조 + 와이어프레임.
- **Evaluator 스프린트 패스**: C1 일부(플로우↔문제 정합) + 와이어프레임 존재 시 `HUMAN_CHECKPOINT_REQUIRED` 표시(시각 부분은 점수화하지 않음, P-1).

### Mode 5 (풀 기획 패키지) 스프린트 적응

Mode 5는 단일 문서가 아니라 ~16종 산출물 세트다. 4개 스프린트는 그대로 돌되 범위가 확장된다:
- **S1**: 경쟁 매트릭스를 `02-market-competition.md`로 정리(확장).
- **S2**: 페르소나를 `03-personas.md`로(복수 가능). 문제 목록은 PRD·유저스토리의 앵커.
- **S3**: IA·유저플로우·화면·**와이어프레임**까지 무겁게 → `12-ia.md`/`13-user-flows.md`/`20-wireframes.md`/`21-screen-spec.md`의 리서치 토대. 와이어프레임 존재 → STEP 7 발동.
- **S4**: **deliverable-그룹별로 확장**된다(통합작성이 단일 문서가 아니라 그룹별 다중 문서). 권장 그룹 분할(각 그룹 = sprint_contract 1건 + Evaluator 패스 1건):
  - S4-g1 발견·전략: 00-business-model(단일 출처 먼저 확정), 01-service-plan, 02, 03
  - S4-g2 정의: 10-prd, 11-user-stories, 12-ia, 13-user-flows
  - S4-g3 설계: 20-wireframes, 21-screen-spec (STEP 7 인간 체크포인트 대상)
  - S4-g4 기술: 30-functional-spec, 31-erd, 32-api-spec, 33-policy (00 참조, 상호 정합)
  - S4-g5 실행: 40-backlog, 41-qa-testcases
  - S4-g6 최종 읽기 문서: `index.html` — 전 산출물을 도식 시각화(마인드맵·다이어그램·순서도·아키텍처·ERD·차트·인포그래픽·웹툰)로 종합한 자체완결 HTML 대시보드. `references/html-visual-template.md` 규칙으로 렌더. g1~g5 전부 PASS + STEP 7 와이어프레임 체크포인트 통과 후 마지막에 생성(새 내용 생성 금지, 기존 산출물 시각화만).
  - **순서 강제**: g1(특히 00-business-model 돈 규칙)이 먼저 확정돼야 g4(ERD·API·정책서)가 그 위에 정합하게 쌓인다. 돈 규칙은 한 곳(00)에서만 정의. g6 HTML은 항상 마지막.

### S4 — 기획서 통합작성 → `service-plan.md` (최종 산출물; Mode 5는 위 그룹별 다중 문서)

- **목적**: S1–S3 리서치를 선택 모드 구조(`mode-templates.md`)로 통합. **신규 리서치 없음, 통합 + 범위/우선순위/지표 확정만.** (Mode 5는 `package/` 다중 문서로 확장.)
- **Feeds**: C3, §8 게이트 G-a/G-d/G-e (그리고 전체 G-f 자리표시자).
- **관측가능 체크 / Definition of Done**:
  - 선택 모드의 모든 섹션이 실제 내용으로 채워졌다(자리표시자 0개 — G-f).
  - 모든 핵심기능이 문제정의의 특정 문제에 매핑됐다(기능↔문제 추적표 — G-a).
  - MVP / Out-of-scope가 명시 분리됐다(G-e), 우선순위에 근거가 있다.
  - 모든 성공지표에 측정가능 수치 + 측정방법(도구/기간/판별 기준)이 붙었다(G-d).
  - 차별화 섹션이 최소 1개 실제 경쟁과 명시 비교한다(G-c, S1 근거 활용).
  - 섹션 간 정합성이 교차 점검됐다.
- **Evaluator 패스**: 이 스프린트 패스 + **STEP 5 최종 통합 패스에서 4축 전부 + 7 probe + §8 6게이트 전부**.

## 스프린트 루프 (각 스프린트 공통)

```
1) Generator: sprint_contract.md 제안 (deliverable + 관측가능 체크 + 출력 파일/포맷, 모드 적응)
2) Evaluator: 계약 승인 또는 수정 (체크가 spec/playbook 바보다 약하면 강화) — 승인 전 빌드 금지
3) Generator: 스프린트 산출물 작성 → self-verify(계약 체크 전부) → generator_report.md → READY_FOR_QA
4) Evaluator: 산출물을 계약 체크 + 스프린트 관련 probe/게이트로 평가 → critique.md → PASS/FAIL
5) PASS → 다음 스프린트 / FAIL & round<cap → 같은 스프린트 재시도(Strategic Decision) / FAIL & round=cap → 미해결 이슈 투명 보고 + 사용자 판단
```

## 반복 캡 (per sprint)

- **5–15 rounds per sprint, 기본 시작 ~8** (범위, 단일값 아님).
- 저점(5)을 고르면 후반 라운드 돌파를 놓칠 수 있음 — "iteration 10 leap" 경고(SKILL.md). 캡은 스프린트별 적용, 전체 런 = 4개 스프린트 합.

## 파일 흐름 (Full file set)

`spec.md`·`sprint-playbook.md`(Planner→) / `sprint_contract.md`(Generator⇄Evaluator, 스프린트별) / `research-s1.md`·`research-s2.md`·`research-s3.md`(스프린트 산출물, 후속 스프린트가 읽음) / `service-plan.md`(S4 최종) / `generator_report.md`·`critique.md`(스프린트별+최종) / `handoff.md`(컨텍스트 압박 시).
