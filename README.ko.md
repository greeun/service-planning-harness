# service-planning-harness

> 🇺🇸 [English README](./README.md)

한두 문장의 서비스 아이디어를 **개발 착수 가능한 서비스 기획서** — 또는 **비주얼 HTML 대시보드까지 포함한 16종 기획 산출물 패키지** — 로 바꿔 주는 Claude Code 스킬. **Full tier**의 3-에이전트 **GAN 하니스**(Planner → Generator → Evaluator)로 동작한다.

Anthropic의 *Harness Design for Long-Running Application Development*의 Planner–Generator–Evaluator 패턴 기반. 생성과 평가를 **별도의 `Agent` 디스패치**로 분리해, 자기평가 편향이 평범한 결과물을 슬쩍 통과시키지 못하게 한다.

---

## 왜 필요한가

그냥 "기획서 써줘"라고 하면 LLM은 자신감 있고 구조적으로는 완결돼 보이지만 얕은 산출물을 낸다 — 차별점은 "더 편리/더 빠름"으로 뭉개지고, 범위는 "전부 다 만들자"로 부풀고, 성공지표는 "만족도 향상"으로 끝난다. 이 스킬은 반대로 강제한다:

- **합성 전에 리서치를 게이트한다.** 4개 리서치 스프린트(시장 → 문제 → UX → 통합)가 각각 적대적 Evaluator를 통과한 **뒤에만** 그 위에 기획서를 쌓는다.
- **Claude 약점축에 2× 가중.** 압박 없으면 Claude가 약한 차별화·실행가능성이 루브릭을 지배한다.
- **모든 주장이 검증 가능.** 6개 이진 게이트(기능↔문제 추적 · 페르소나 구체성 · 실명 경쟁 비교 · 지표 측정성 · MVP/비범위 분리 · 자리표시자 0)가 전부 통과해야 한다.

---

## 동작 방식

```
[메인 세션]  ── 인테이크: 어떤 모드? + 1~4문장 아이디어
      │
      ▼
STEP 3  Planner  ─────►  spec.md + sprint-playbook.md   (모드별 구조)
      │
      ▼
STEP 4  4개 리서치 스프린트 (각: 계약 → 빌드 → 평가, cap 5~15)
      │   S1 시장·경쟁 → research-s1.md
      │   S2 문제·타겟·페르소나 → research-s2.md
      │   S3 UX/UI 플로우·화면 → research-s3.md
      │   S4 통합 → service-plan.md  (Mode 5: 그룹 g1~g6 → package/)
      ▼
STEP 5  최종 통합 Evaluator 패스  (4축 루브릭 + 7 probe + 6 게이트)
      │
      ▼
STEP 7  조건부 와이어프레임 인간 체크포인트  (와이어프레임 있을 때만 발동)
      ▼
STEP 7.5  비주얼 HTML 대시보드 렌더  (index.html)
      ▼
STEP 7.6  중요도 그룹핑 길잡이  (INDEX.md — 티어 · 작업순서 · 역할별 · 최소 착수 세트)
      ▼
STEP 8  인도
```

Generator와 Evaluator는 서로의 추론을 보지 못한다 — 오직 파일로만 소통한다(`spec.md`, `sprint_contract.md`, `research-s*.md`, `critique.md`, `handoff.md`).

---

## 출력 모드

활성화 시 항상 어떤 모드를 만들지 먼저 묻는다(STEP 1, 필수 게이트):

| 모드 | 이름 | 산출물 |
|------|------|--------|
| 1 | **Lean MVP 기획서** *(기본)* | 문제 → 타겟 → 핵심기능 → 유저플로우 → MVP범위 → 지표. 약 4주 착수 가능. S3 라이트, 와이어프레임 없음. |
| 2 | **정식 PRD** | + 기능명세 / 화면정의 / 엣지케이스 / 비기능요구. |
| 3 | **비즈니스+기획 통합** | + 시장 / 경쟁 / 수익모델 분석. S1 확장. |
| 4 | **화면·기능 명세 중심** | 유저플로우 / 화면별 기능 / 데이터 구조. S3 최중량 + 와이어프레임. |
| 5 | **풀 기획 패키지** | 5단계 ~16종 산출물 **+ 비주얼 HTML 대시보드**(`index.html`) **+ 중요도 그룹핑 길잡이**(`INDEX.md`). |

### Mode 5 — 풀 패키지

```
package/
├─ 발견·전략   00-business-model · 01-service-plan · 02-market-competition · 03-personas
├─ 정의        10-prd · 11-user-stories · 12-ia · 13-user-flows
├─ 설계        20-wireframes · 21-screen-spec
├─ 기술        30-functional-spec · 31-erd · 32-api-spec · 33-policy
├─ 실행        40-backlog · 41-qa-testcases
├─ 최종 읽기    index.html   (마인드맵 · 순서도 · 아키텍처 · ERD · 차트 · 인포그래픽 · 웹툰 패널)
└─ 길잡이       INDEX.md     (중요도 티어 · 작업순서 · 역할별 묶음 · 최소 착수 세트)
```

**단일 출처 원칙**을 강제한다: 돈 규칙(가격·수수료율·정산 트리거)은 `00-business-model.md` 한 곳에서만 정의하고, ERD·API·정책서는 그것을 인용만 하며 절대 재정의하지 않는다.

---

## 평가 루브릭

| # | 기준 | 가중 | 이유 |
|---|------|------|------|
| C1 | 문제정의 명확성 | 1× | Claude가 기본으로 잘 채움; 타겟 구체성 게이트로 보강. |
| C2 | **차별화·독창성** | **2×** | Claude 약점-기본축 — 압박 없으면 table-stakes 일반론으로 흐름. |
| C3 | **실행가능성(MVP범위)** | **2×** | Claude는 과대범위로 흐름; MVP/비범위 명시 분리 필요. |
| C4 | 성공지표 측정가능성 | 1× | 섹션 골격은 기본 적정; 측정성 게이트로 보강. |

**판정:** 모든 기준 ≥4, 적대적 probe 전부 clean, §8 6게이트 전부 통과 → **PASS**. 2×축 중 하나라도 <4 → **FAIL**. 채점은 `references/evaluator-calibration.md`의 구체적 1/3/5 few-shot 앵커로 보정한다.

---

## 비주얼 HTML 대시보드

최종 산출물은 모든 문서를 **자체완결 + 도식 시각화 중심 HTML 대시보드**로 렌더해, 기획 전체를 한눈에 파악하게 한다 — 마인드맵, 가치순환·자금흐름 인포그래픽, 경쟁 매트릭스, 웹툰형 페르소나 패널, 유저플로우 차트, IA 트리, 시스템 아키텍처, ERD, 지표 게이지, 범위 인포그래픽, 백로그 간트. 다이어그램은 Mermaid, 인포그래픽은 인라인 SVG/CSS(오프라인 동작). `references/html-visual-template.md` 참조.

---

## 중요도 그룹핑 (INDEX.md)

모든 프로젝트가 16종 전부 필요한 건 아니다. 패키지 작성이 끝나면 Mode 5는 `INDEX.md`를 생성해 산출물을 **나열이 아니라 분류**한다:

- **중요도 티어** — T1 *필수*(없으면 개발 착수 불가) / T2 *조건부*(도메인 따라 필수 — 예: 결제·규제 서비스면 `33-policy`·`00-business-model`) / T3 *보조*(정렬·설득·온보딩). 티어는 고정이 아니라 서비스별로 판정한다.
- **작업 순서** — 의존성 기반 읽기/작성 순서(돈 규칙 먼저 → 왜/누구 → 무엇 → 화면 → 데이터/API → 실행 → 시각 종합).
- **역할별 묶음** — 개발자 / QA / 디자이너 / PO / 경영이 각자 실제로 읽는 부분만.
- **최소 착수 세트** — 개발 시작에 충분한 린 ~6종(`10-prd` · `13-user-flows` · `21-screen-spec` · `31-erd` · `32-api-spec` · `40-backlog`). 16종 전부는 킥오프 정렬·고객 납품·규제 도메인용이다.

이로써 패키지가 "문서를 위한 문서"가 되는 것을 막는다 — *무엇부터 읽고, 누가 무엇을 읽는지*를 알려 준다.

---

## 사용법

트리거 문구:

- **한국어:** "서비스 기획", "기획서 작성", "서비스 기획서", "PRD 작성", "MVP 기획", "제품 기획", "서비스 기획 스킬", "화면 명세 작성", "전체 기획 산출물", "기획 패키지"
- **English:** "service planning", "write PRD", "product spec", "MVP plan", "full planning package"

코드 디버깅·콘텐츠 작성에는 **발동하지 않는다** — 기획 산출물 생성에만 활성화된다.

---

## 디렉터리 구조

```
service-planning-harness/
├─ SKILL.md                          # 오케스트레이터: 8스텝 플로우, 모드 게이트, 스프린트 루프, 튜닝 루프
└─ references/
   ├─ planner-prompt.md              # Planner: 모드 선택 + spec/sprint-playbook 작성
   ├─ generator-prompt.md            # Generator: 스프린트별 계약 → 빌드 → self-verify
   ├─ evaluator-prompt.md            # Evaluator: 스프린트별 + 최종 패스, 7 적대적 probe
   ├─ evaluator-calibration.md       # C1~C4 few-shot 1/3/5 앵커
   ├─ rubric.md                      # 4 기준, 2× 가중, 판정 로직
   ├─ sprint-playbook.md             # 4 리서치 스프린트 + Mode 5 그룹 확장(g1~g6)
   ├─ mode-templates.md              # 5 출력 모드 문서 골격
   └─ html-visual-template.md        # 비주얼 HTML 대시보드 렌더 규칙 + 스켈레톤
```

---

## 티어 선택 (Full vs Simplified)

이 스킬은 컨텍스트 불안이 사라진 강모델에서도 **의도적으로 Full tier**(스프린트 계약 + 스프린트별 Evaluator 패스)로 돈다. 이유는 컨텍스트 관리와 직교한다: 서비스 기획은 **리서치-페이즈 분해 + 페이즈별 품질 게이팅**을 요구한다 — 약한 시장조사나 모호한 문제정의는 그 위에 기획서가 합성되기 **전에** 잡아내야 한다.

| 모델 | 컨텍스트 불안 | 기본 티어 | 이 스킬 |
|------|---------------|-----------|---------|
| Sonnet 4.5 | 강함 | Full | Full |
| Opus 4.5 | 대부분 사라짐 | Simplified | Full (리서치 게이팅) |
| Opus 4.6 | 사라짐 | Simplified / Single-session | Full (리서치 게이팅) |
| Opus 4.8 | 사라짐 (1M ctx) | Simplified | **Full (의도적)** |

향후 모델 업그레이드 시 스트레스 테스트: *명시 sprint contract 없이 모델이 4개 리서치 페이즈를 스스로 조직하고 품질을 self-gate할 수 있는가?* 가능하면 스프린트를 **한 번에 하나씩** 제거한다 — 전부 한꺼번에 제거하지 않는다.

---

## 예시 실행

**character-market**(AI 캐릭터 + 창작자 직접 업로드 양면 마켓플레이스)에 Mode 1 → Mode 5로 종단 테스트했다. 4개 리서치 스프린트와 최종 통합 패스 모두 **PASS**(C1 5 · C2 5 · C3 5 · C4 4; 6게이트 전부 통과). 풀 패키지(16종 + 비주얼 대시보드)는 프로젝트의 `planning/package/`에 있다.

---

## 설계 원칙

1. **GAN 역할분리** — Generator와 Evaluator는 별도 `Agent` 디스패치. 한 세션이 자기 결과를 채점하면 평범함을 자신 있게 칭찬한다.
2. **파일 기반 핸드오프만** — 에이전트는 추론을 공유하지 않고 파일로만 소통.
3. **작업 전 스프린트 계약** — Evaluator가 각 스프린트의 관측가능 체크를 승인한 뒤에만 Generator가 빌드.
4. **컴팩션 말고 리셋** — 컨텍스트 압박은 `handoff.md` + fresh 세션으로 회복, 컴팩션 금지.
5. **근거 있는 루브릭** — 모든 점수가 정확한 인용 + 위치를 댄다; "괜찮아 보임"은 통과 못 함.
6. **모든 컴포넌트는 가정을 인코딩** — 모델 업그레이드 시 각 가정을 스트레스 테스트하고 load-bearing 아닌 것은 제거.

> Anthropic, *Building Effective Agents*: "find the simplest solution possible, and only increase complexity when needed."

---

## 크레딧

[`skill-wizard-harness`](../skill-wizard-harness)로 생성·감사했다. 이 위자드 자체가 Planner–Generator–Evaluator 메타패턴의 적용 사례다.
