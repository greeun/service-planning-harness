# Evaluator Calibration — service-planning-harness

> Few-shot scoring calibration anchors. Before scoring, the Evaluator reads this file and aligns each criterion's 1/3/5 score to these anchors. This aligns scoring with the judgment of a demanding senior PM / planning lead and reduces score drift between rounds.
> The anchors are concrete examples from the Korean service-planning domain. **Do not change** — apply them as-is.
> Operational tuning (read the critique log, add missing patterns as counterexamples) follows the SKILL.md "Evaluator tuning workflow (a)–(d)" procedure. Update this file when adding a new counterexample there.

---

## C1 problem-definition clarity

- **1/5**: "A scheduling app for office workers. People find managing their schedules hard." ("직장인을 위한 일정관리 앱. 사람들이 일정 관리를 어려워한다.") — who/why is abstract, no persona or situation.
- **3/5**: "A freelance designer can't see deadlines for multiple clients in one place and misses deadlines." ("프리랜서 디자이너가 여러 클라이언트의 마감을 한 곳에서 못 봐서 마감을 놓친다.") — target and problem are clear, but the frequency/context of the pain point is shallow.
- **5/5**: "A freelance designer running 3+ simultaneous projects per week (e.g., Kim, age 32) manually collects deadlines scattered across KakaoTalk, email, and Notion every day, and misses a deadline about twice a month, causing rework and loss of trust." ("주 3개 이상 동시 프로젝트를 진행하는 프리랜서 디자이너(예: 만 32세 김OO)는 카톡·이메일·노션에 흩어진 마감을 매일 수동 취합하다 월 2회꼴로 마감을 놓쳐 재작업·신뢰 손실을 겪는다.") — sharp down to persona, situation, frequency, and consequence.

## C2 differentiation/originality

- **1/5**: "More convenient and faster than existing services." ("기존 서비스보다 더 편리하고 빠릅니다.") — no competitor named, no differentiation axis (table-stakes generality).
- **3/5**: "It has an automatic deadline-aggregation feature compared to Notion/Trello." ("노션·트렐로 대비 마감 자동 취합 기능이 있다.") — names competitors but the comparison axis is single and there's no argument about imitation difficulty.
- **5/5**: "Compared to Notion (general-purpose workspace, no automatic deadline aggregation) and Trello (board-centric, no external-channel integration), the core differentiator is automatically parsing deadlines from KakaoTalk, email, and Notion into a single calendar, and this parsing pipeline becomes a 6-month head-start moat." ("노션(범용 워크스페이스, 마감 자동집계 없음)·트렐로(보드 중심, 외부 채널 연동 없음) 대비, 카톡·이메일·노션 마감을 자동 파싱해 단일 캘린더로 통합하는 것이 핵심 차별점이며, 이 파싱 파이프라인이 6개월 선점 해자가 된다.") — multiple competitors, comparison axes, moat argument.

## C3 feasibility (MVP scope)

- **1/5**: "Implement all features in the first phase." ("1차로 모든 기능을 다 구현한다.") — no prioritization or scope separation, not startable.
- **3/5**: "MVP is deadline registration and a calendar view. Add automatic parsing later." ("MVP는 마감 등록과 캘린더 뷰. 이후 자동 파싱 추가.") — scope is narrowed, but out-of-scope is vague and the prioritization rationale is weak.
- **5/5**: "MVP (4 weeks): manual deadline registration + integrated calendar view + D-1 deadline alert. Out-of-scope (explicitly excluded): automatic parsing, team collaboration, payments. Automatic parsing is the core differentiator but carries high technical risk, so it is split into phase 2 after MVP validation." ("MVP(4주): 수동 마감 등록 + 통합 캘린더 뷰 + 마감 D-1 알림. Out-of-scope(명시 제외): 자동 파싱·팀 협업·결제. 자동 파싱은 핵심 차별점이나 기술 리스크가 커 MVP 검증 후 2차로 분리.") — explicit scope/out-of-scope, prioritization rationale, startable.

## C4 success-metric measurability

- **1/5**: "Increase user satisfaction." ("사용자 만족도를 높인다.") — no measurement number or method.
- **3/5**: "Reach 1,000 MAU after launch." ("출시 후 MAU 1,000 달성.") — has a number but the measurement method/period/criterion is incomplete.
- **5/5**: "Within 8 weeks of launch, weekly retention (W4) of 25%+ (measured via Amplitude cohorts); reduce deadline-miss rate per user from 2/month to 0.5/month (in-app survey N=50, pre/post comparison)." ("출시 8주 내 주간 리텐션(W4) 25% 이상(앰플리튜드 코호트 측정), 마감 미스율 사용자당 월 2회→0.5회 감소(인앱 설문 N=50, 사전·사후 비교).") — number + measurement tool + period + criterion.

---

## Usage rules

- When scoring each criterion, **directly compare** the relevant part of the deliverable against the 1/3/5 anchors above. As in: "Is this persona better than the C1 3/5 anchor, as sharp as the 5/5 anchor?"
- For in-between values (2/4), decide by which adjacent anchor it is closer to.
- If every axis looks ≥4, consider PASS only after applying rubric.md "Calibration Checkpoint" (domain-expert/competitor/user lenses).
- When you find a new failure pattern (e.g., overrating a metric that looks measurable but lacks a discrimination criterion at 3/5), add a counterexample to this file per SKILL.md tuning (c).
