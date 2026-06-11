# CLAUDE.md

이 프로젝트에서 Claude가 따라야 할 행동 규칙.

## 저장소

GitHub: `kyoo111/wire-briefing`
gh CLI 경로: `C:\Program Files\GitHub CLI\gh.exe`

## 프로젝트 개요

신선기(Wire Drawing Machine) · 열처리 공정 설비담당자를 위한 일일 브리핑 자동 생성.
- 대상: 경강선 제조 설비 (신선기, 파텐팅로 등)
- 수집 범위: 전세계 (한국·일본·독일·영어권 등)
- 배포: 매일 오전 8시 HTML 파일 생성

## 파일 경로

| 파일 | 경로 |
|---|---|
| 스케줄 태스크 프롬프트 (실행) | `C:\Users\Admin\.claude\scheduled-tasks\wire-briefing\SKILL.md` |
| 스케줄 태스크 프롬프트 (백업) | `C:\Users\Admin\Desktop\wire_briefing\scheduled-task-prompt.md` |
| 브리핑 저장 경로 | `C:\Users\Admin\Desktop\wire_briefing\reports\YYYY-MM-DD_wire_report.html` |
| HTML 템플릿 | `C:\Users\Admin\Desktop\wire_briefing\templates\wire_report.html` |

## 스케줄 태스크 프롬프트 수정 시 규칙

두 파일을 항상 동일하게 유지한다.
수정 후 반드시 복사:
```powershell
Copy-Item "C:\Users\Admin\.claude\scheduled-tasks\wire-briefing\SKILL.md" `
  -Destination "C:\Users\Admin\Desktop\wire_briefing\scheduled-task-prompt.md" -Force
```

## 커밋 규칙

```
feat: 새 기능 추가
fix: 이슈 수정 (closes #N)
docs: 문서 수정
chore: 설정·템플릿 수정
```
