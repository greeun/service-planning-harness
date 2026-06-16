# HTML Visual Template — service-planning-harness

> 최종 산출 단계에서, 생성된 기획 산출물(.md)을 **읽기 쉬운 + 도식 시각화 중심의 자체완결 HTML 대시보드**로 렌더한다.
> 목표: 모든 산출물의 **구조를 한눈에**. 본문 복붙이 아니라 마인드맵·다이어그램·순서도·아키텍처·차트·인포그래픽·웹툰 패널로 구조와 핵심을 시각화하고, 상세는 접이식으로 둔다.

## 언제 / 무엇을

- **Mode 1~4 (단일 문서)**: `<service>-plan.html` 1파일 — 해당 모드 문서를 섹션화 + 모드에 맞는 시각화(아래 카탈로그에서 해당되는 것만).
- **Mode 5 (풀 패키지)**: `package/index.html` 1파일 — 5단계 16종을 시각화 카탈로그 13종으로 종합한 대시보드. (오케스트레이터: STEP 7 와이어프레임 체크포인트 직후, STEP 8 인도 전에 렌더.)
- 렌더 입력 = 이미 PASS된 최종 산출물(.md). HTML은 **새 내용 생성 금지** — 기존 산출물의 구조·수치·관계만 시각화한다(자리표시자 0, 수치는 산출물·`00-business-model.md`에서만 인용).

## 기술 방침 (자체완결 1파일)

- 단일 `.html`. CSS/JS 인라인. 외부 의존은 **다이어그램용 Mermaid.js만** 허용(CDN: `https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js`, ESM import + `mermaid.initialize({startOnLoad:true, theme:'neutral'})`). 인포그래픽·차트·웹툰은 **인라인 SVG/CSS**(오프라인 동작).
- 상단에 한 줄 주석: "다이어그램은 인터넷 필요(Mermaid CDN), 인포그래픽/차트는 오프라인 동작."
- (완전 오프라인 요구 시: Mermaid를 인라인 번들로 넣거나, 다이어그램도 인라인 SVG로 대체.)
- 절제된 팔레트 1 액센트(예: 인디고 #4f46e5 + 보조 틸). 다크 텍스트·라이트 배경. 시스템 폰트. 슬롭 금지 — 의도적으로 디자인된 인터널 대시보드 수준(여백·위계·스캔성).

## 시각화 카탈로그 (산출물 → 시각화 → 기술)

| # | 시각화 | 소스 문서 | 기술 |
|---|--------|-----------|------|
| 1 | **전체 구조 마인드맵** (서비스→단계→문서/기능) | 전체 / PRD | Mermaid `mindmap` — 히어로에 배치, "한눈에 전체 구조" |
| 2 | **핵심 루프/메커니즘 다이어그램** (예: 양면 가치순환) | 페르소나/기획서 | 인라인 SVG 원형 루프 (화살표·라벨) |
| 3 | **수익/자금 흐름 인포그래픽** | 00-business-model | 인라인 SVG (split·풀 분배·금액·Phase 태그) |
| 4 | **유닛이코노믹스 차트** (분해·BEP) | 00-business-model | 인라인 SVG bar/donut |
| 5 | **경쟁 매트릭스 + 포지셔닝 산점도** | 02-market-competition | CSS heatmap(✓/△/✗ 컬러칩, 자사 행 강조) + 인라인 SVG 2축 scatter |
| 6 | **페르소나 웹툰 패널** (상황→트리거→페인→해결) | 03-personas | CSS 만화 컷 + 말풍선/생각풍선 |
| 7 | **유저플로우 순서도** (역할별 경로, 노드에 화면·기능 태그) | 13-user-flows | Mermaid `flowchart` (Phase2 점선) |
| 8 | **IA 화면 트리** (화면ID 위계, 역할별 색) | 12-ia | Mermaid `flowchart TD` / graph |
| 9 | **시스템 아키텍처** (클라이언트→API→데이터→외부 레이어) | 30/31/32 | Mermaid `flowchart TB` |
| 10 | **ERD 다이어그램** (핵심 엔티티·관계) | 31-erd | Mermaid `erDiagram` (전체 엔티티 목록은 접이식) |
| 11 | **성공지표 게이지/바 차트** (목표값) | 01/06 지표 | 인라인 SVG/CSS, 축별 색 |
| 12 | **MVP vs Phase2 범위 인포그래픽** | 01/10 | CSS 2열 VS (포함/제외 색 구분) |
| 13 | **백로그 간트** (스프린트 배치, 임계경로) | 40-backlog | Mermaid `gantt` |
| (도메인 추가) | 시퀀스·상태도·퍼널·히트맵 등 | 해당 문서 | Mermaid / 인라인 SVG — 도메인에 맞게 창의적으로 |

> Mode 1~4는 위 중 해당되는 것만(예: Mode 1 Lean → 1·2·5·7·11·12). Mode 5는 전부 + 도메인 추가.

## 레이아웃

- 고정 좌측 **사이드바 TOC**(단계/문서 그룹, 점프) + 메인 스크롤(콘텐츠 max-width ~960px, line-height 1.7). 좁은 화면(<900px) 햄버거 토글.
- 스티키 상단바: 서비스명 + 한줄 요약 + 메타(작성일·산출물 수) + `[🖨 인쇄/PDF]`(window.print).
- 히어로: 한줄 가치 + 스탯카드(모드·산출물 수·MVP 기간·수익모델 한줄) + **전체 구조 마인드맵(#1)**.
- 단계별 섹션: 각 섹션에 해당 시각화 + 그 아래 **접이식(`<details>`) 상세 핵심**(표·리스트·배지).
- **배지**: 우선순위 P0(빨강)·P1(주황)·P2(회색), Phase MVP(초록)·Phase2(회색). ID 토큰(`F1`·`SC-02`·`P3`·`T1`·`E12`)은 monospace 칩으로 자동 스타일.
- 인터랙션: 사이드바 active highlight(IntersectionObserver), 스무스 스크롤, 접이식 그룹.

## 자체완결 스켈레톤 (슬롯 채워 사용)

```html
<!doctype html><html lang="ko"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{TITLE}} — 기획 대시보드</title>
<style>
/* 변수: --accent, --accent2, --bg, --fg, --muted, --line; 배지/칩/카드/표/웹툰/프린트 CSS */
/* 레이아웃: .layout{display:grid;grid-template-columns:260px 1fr} .sidebar{position:sticky;top:0;height:100vh;overflow:auto} */
/* 표: 래퍼 overflow-x:auto, zebra, sticky thead; pre: monospace 보존; .callout 강조 박스 */
/* @media print{ .sidebar,.topbar,button{display:none} details{open} *{break-inside:avoid} main{width:100%} } */
/* @media (max-width:900px){ .layout{grid-template-columns:1fr} .sidebar{position:fixed;transform:translateX(-100%)} .sidebar.open{transform:none} } */
</style></head>
<body>
<header class="topbar">{{TITLE}} · {{ONELINE}} · {{META}} <button onclick="window.print()">🖨 인쇄/PDF</button></header>
<div class="layout">
  <nav class="sidebar" aria-label="목차">{{TOC}}</nav>
  <main>
    <section id="hero">{{ONELINE}} {{STAT_CARDS}} <div class="mermaid">{{MINDMAP}}</div></section>
    {{MONEY_CALLOUT}}
    {{SECTIONS}}  <!-- 단계별: 시각화(svg/mermaid/css) + <details> 상세 -->
  </main>
</div>
<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js";
  mermaid.initialize({startOnLoad:true, theme:"neutral", securityLevel:"loose"});
  // IntersectionObserver active TOC highlight; 햄버거 토글; beforeprint→ details[open]=true
</script>
</body></html>
```

## 규칙 (audit 시 확인)

- **시각화가 주연**: 카탈로그의 해당 항목을 **실제 데이터로** 채운다(빈 다이어그램·자리표시자 금지).
- **내용 충실·무생성**: 산출물의 구조·수치·관계만 시각화. 돈 규칙은 `00-business-model.md` 인용만(재정의 금지). 산출물에 없는 수치 발명 금지.
- **자체완결**: 외부 의존은 Mermaid CDN 1개만. 더블클릭으로 열림. 인포그래픽은 오프라인 동작.
- **프린트**: 사이드바/버튼 숨김, 접이식 펼침, page-break-inside avoid, Mermaid 렌더 SVG 인쇄.
- **접근성**: 시맨틱 랜드마크(nav/main/header), aria 토글, 대비 충분, 포커스 스타일.
- **한글**: 표·칩 word-break 안전, 오버플로 클리핑 없음.
- 의도적으로 디자인된 대시보드 — 제네릭 부트스트랩 덤프 금지.

## 참고 인스턴스

`character-market/planning/package/index.html` (Mode 5, 시각화 13종, 1파일 ~86KB) — 이 템플릿의 실제 적용 예.
