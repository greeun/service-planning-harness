# HTML Doc Template — service-planning-harness

> 산출물 `.md`를 **읽기·공유·리뷰가 쉬운 자체완결 HTML**로 추가 렌더한다. **원본 `.md`는 항상 보존**한다(HTML은 추가 산출).
> 두 종류의 HTML이 있다(혼동 금지):
> - **이 템플릿(html-doc-template)** = 문서별 **읽기용 HTML** + 허브 `index.html`. 가독성·공유·리뷰 목적. 본문을 그대로 읽기 좋게 렌더.
> - `html-visual-template.md` = **도식 종합 대시보드**(마인드맵·차트·인포그래픽). "한눈에 구조" 목적. "둘 다" 선택 시 `overview.html`로 함께 산출.

## 언제 (STEP 1에서 사용자가 선택)

활성화 STEP 1에서 모드와 함께 **HTML 산출 형태**를 묻는다:
- **문서별 HTML + 허브 index.html** [권장] — 이 템플릿. md 보존 + 각 문서 읽기용 html + 허브.
- **둘 다** — 위 + 도식 대시보드 `overview.html`(`html-visual-template.md`).
- **비주얼 대시보드만** — `index.html` = 도식 대시보드만(`html-visual-template.md`).
- **없음(md만)** — HTML 생략.

선택을 `spec.md` / `sprint-playbook.md`에 동결한다. 기본 제안은 "문서별 HTML + 허브".

## 산출 구조 (문서별 HTML + 허브)

```
docs/                         (Mode 5는 docs/, Mode 1~4는 단일 폴더)
├─ index.html                     허브: 문서 목록·중요도 티어·작업순서·각 문서 링크
├─ 00-business-model.md  / .html  원본 md 보존 + 읽기용 html (Mode 5 예)
├─ 01-service-plan.md    / .html
├─ ... (각 .md 옆에 같은 이름 .html)
├─ INDEX.md                        텍스트 길잡이(허브 index.html의 소스)
└─ overview.html                   ("둘 다" 선택 시만) 도식 종합 대시보드
```
Mode 1~4(단일 문서)는 `service-plan.md` + `service-plan.html`만(허브 불필요, 단 원하면 생성).

## 기술 방침 (자체완결 1파일씩)

- 각 html은 **외부 의존 0**(인라인 CSS/JS만, CDN·폰트·플러그인 금지). 더블클릭으로 열림, 오프라인·이메일 첨부·사내망 공유 OK.
- 시스템 폰트. 절제된 팔레트 1 액센트. 본문 가독 우선(max-width ~820px, line-height 1.7).
- **신규 내용 생성 금지** — `.md` 본문을 충실히 HTML로 변환만(요약·삭제·창작 금지).

## 문서별 읽기 HTML (`NN-name.html`) 규칙

- 상단 바: 문서 제목 + 문서 번호/티어 배지 + `[← 목록]`(index.html) + `[← 이전 / 다음 →]` 문서 링크 + `[🖨 인쇄/PDF]`.
- 본문: md → HTML 충실 변환. 헤딩→h2/h3/h4(앵커 id), 표→스타일 테이블(가로 스크롤 래퍼), 리스트, 코드→monospace 블록, 인용→callout. 코드/ID 토큰(`F1`·`SC-02`·`P3`) monospace 칩.
- 우측/좌측 **목차(TOC)**: 그 문서의 헤딩, 스크롤 시 현재 위치 하이라이트(IntersectionObserver), 클릭 점프.
- **리뷰 편의**: 각 h2/h3에 `#앵커` (특정 섹션 URL 공유), 섹션 옆 §링크. (선택) 문단 hover 시 앵커 표시.
- 단일출처 주의: 돈 규칙 등은 `00-business-model`을 인용만(재정의 금지) — md와 동일.
- 프린트 CSS: 상단바·버튼·TOC 숨김, 본문 full-width, page-break-inside avoid.
- 접근성: 시맨틱 랜드마크, 대비, 포커스.

## 허브 (`index.html`) 규칙

`INDEX.md`(중요도 그룹핑)를 **그대로 시각화한 네비게이션 페이지**다. 신규 분류 만들지 말고 INDEX.md를 따른다.
- 헤더: 서비스명 + 한 줄 요약 + 메타(작성일·문서 수).
- **중요도 티어별 카드**: T1 필수 / T2 조건부 / T3 보조 — 각 티어에 문서 카드(번호·이름·한 줄 용도·`NN-name.html` 링크·티어 배지).
- **작업 순서**: 번호 매긴 읽기/작성 순서(각 항목이 해당 html로 링크).
- **역할별 묶음**: 개발/QA/디자이너/기획/경영 탭 또는 섹션 — 각 역할이 볼 문서만 링크.
- **최소 착수 세트**: 강조 박스 + 해당 문서 링크.
- ("둘 다" 선택 시) 상단에 `[📊 도식 대시보드 보기 → overview.html]` 버튼.
- 자체완결(인라인 CSS/JS), 프린트 CSS, 반응형(좁은 화면 카드 1열).

## 스켈레톤 (문서별 html — 슬롯 채워 사용)

```html
<!doctype html><html lang="ko"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{DOC_TITLE}} · {{SERVICE}}</title>
<style>/* 변수·타이포·표(zebra,래퍼)·칩·callout·TOC·@media print(상단바/TOC 숨김)·반응형 */</style></head>
<body>
<header class="topbar">{{DOC_NO}} {{TIER_BADGE}} · {{DOC_TITLE}}
  <nav><a href="index.html">← 목록</a> <a href="{{PREV}}">← 이전</a> <a href="{{NEXT}}">다음 →</a>
  <button onclick="window.print()">🖨 인쇄/PDF</button></nav></header>
<div class="layout"><nav class="toc">{{TOC}}</nav>
  <main>{{BODY_HTML}}</main></div>
<script>/* IntersectionObserver TOC 하이라이트, 스무스 스크롤, 헤딩 앵커 */</script>
</body></html>
```

## 규칙 (audit 시 확인)

- **md 보존**: 모든 `.md`가 그대로 남아 있어야 한다(삭제·변형 금지). html은 추가물.
- **충실 변환**: html 본문이 md 내용을 누락·요약·창작 없이 담는다. 표·코드·리스트 보존.
- **자체완결**: 외부 의존 0, 더블클릭으로 열림.
- **허브 = INDEX.md 반영**: 티어·순서·역할·최소세트가 INDEX.md와 일치, 모든 링크 유효(상대경로).
- **프린트/접근성/반응형** 충족.
- Mode 1~4는 단일 문서 html, Mode 5는 문서별 html + 허브(+선택 overview.html).

## 참고

- 도식 종합 대시보드는 `html-visual-template.md` 참조("둘 다"의 `overview.html`).
- 중요도 그룹핑 소스는 `mode-templates.md`의 "INDEX.md 생성 규칙".
