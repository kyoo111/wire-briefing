---
name: briefing-reviewer
description: >
  신선기·열처리 설비 일일 브리핑(wire-briefing)의 약점을 검증하고,
  발견된 문제를 GitHub 이슈로 등록한 뒤, 이슈별 계획 댓글 → HTML 수정 →
  커밋·푸시까지 자동 처리하는 스킬.

  "브리핑 검증해줘", "버그 찾아줘", "이슈 등록해줘", "이슈 처리해줘",
  "약점 찾아줘", "보고서 점검해줘" 등의 말이 나오면 반드시 이 스킬을 사용할 것.
  wire-briefing 관련 품질 확인 요청은 모두 이 스킬로 처리한다.
---

## 개요

오늘 생성된 브리핑을 4가지 기준으로 검증하고, 문제를 이슈로 등록한 뒤 수정까지 완료한다.
"검증만 해줘"라고 하면 1단계까지, "이슈 등록까지 해줘"라고 하면 2단계까지,
"전부 처리해줘"라고 하면 3단계까지 실행한다. 별도 지시가 없으면 3단계 전체를 실행한다.

---

## 1단계 — 브리핑 검증

### 파일 위치
`C:\Users\Admin\Desktop\wire_briefing\reports\` 에서 오늘 날짜 파일을 읽는다.
파일명 형식: `YYYY-MM-DD_wire_report.html`

### 검증 기준 (4가지)

| # | 기준 | 판단 방법 |
|---|---|---|
| 1 | **30일 초과 자료** | 각 항목의 발행일이 오늘 기준 30일 초과 여부 확인 |
| 2 | **업무 무관 항목** | 신선기·열처리 설비와 직접 관련 없는 항목 (제조업 일반 트렌드, 타 산업 뉴스 등) |
| 3 | **출처 없는 수치** | 본문 항목에서 근거를 확인할 수 없는 수치가 헤더/요약에 포함된 경우 |
| 4 | **동일 기업·행사 중복** | 같은 기업 또는 행사 기반 항목이 2건 이상 등장 |

### 출력 형식
각 약점을 아래 형식으로 정리한다.

```
[Bug N] 유형: (30일 초과 / 업무무관 / 출처없는수치 / 중복)
대상: 항목 제목
내용: 문제 설명 1~2줄
심각도: 높음 / 중간 / 낮음
```

---

## 2단계 — GitHub 이슈 등록

검증에서 발견된 약점마다 이슈를 1건씩 등록한다.

```powershell
& "C:\Program Files\GitHub CLI\gh.exe" issue create `
  --repo kyoo111/wire-briefing `
  --title "이슈 제목" `
  --body $body
```

- 제목: 문제를 한 줄로 요약 (유형 + 핵심 내용)
- 본문: `## 문제` + `## 수정 방향` 두 섹션으로 구성
- 등록 후 이슈 번호와 URL을 기록해둔다

---

## 3단계 — 이슈별 처리

각 이슈를 순서대로 처리한다. 이슈가 여러 개이면 HTML 수정을 모아서 한 번에 커밋해도 된다.

### 처리 순서 (이슈 1건당)

**① 처리 계획 댓글 등록**
```powershell
& "C:\Program Files\GitHub CLI\gh.exe" issue comment N `
  --repo kyoo111/wire-briefing `
  --body $planBody
```
댓글 내용: 제거/교체 대상 명시 + 수정 방향 + 예정 커밋 메시지

**② HTML 수정**
- `C:\Users\Admin\Desktop\wire_briefing\reports\YYYY-MM-DD_wire_report.html` 직접 편집
- 문제 항목 제거 또는 교체
- 교체 시 30일 이내 발행된 신선기·열처리 관련 항목으로 대체 (필요 시 웹 검색)

**③ 커밋·푸시**
```powershell
cd C:\Users\Admin\Desktop\wire_briefing
git add reports/YYYY-MM-DD_wire_report.html
git commit -m "fix: 수정 내용 요약 (closes #N)"
git push
```
- 여러 이슈를 한 커밋으로 처리할 때: `closes #2, closes #3, closes #4`
- `closes #N` 포함 시 GitHub이 자동으로 이슈를 Close 처리한다

---

## 주의사항

- 이슈 본문에 백틱(\`)이 포함되면 PowerShell here-string(`@'...'@`)을 사용한다
- 대체 항목 검색 시 발행일이 30일 이내인지 반드시 확인한다
- 동일 기업 중복 처리 시 기술 내용이 더 구체적인 1건을 유지한다
