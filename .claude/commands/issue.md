---
description: Create a GitHub issue from conversation context — auto-detects scope, picks bug or feature template, composes title in Conventional Commits form, opens via gh.
argument-hint: '<bug|feat> [short topic]'
---

# /issue — GitHub 이슈 열기

> 대화 컨텍스트로부터 GitHub issue를 작성하고 gh로 생성한다.

## Arguments

```
/issue <type> [topic...]

type:
  bug | fix     → bug 이슈 (label: bug)
  feat | feature → feature 이슈 (label: enhancement)
```

`type` 누락 시 사용자에게 질문.

## Preflight

```bash
gh auth status
gh repo view --json nameWithOwner
```

## Scope 추론

이 프로젝트의 scope enum:
`map` | `vehicle` | `dashboard` | `stream` | `video` | `event` | `auth` | `ui` | `store` | `api` | `i18n` | `ci` | `docs` | `deps`

추론 우선순위:

1. 최근 편집/언급된 파일 경로 → 매핑
   - `src/components/map/**` → `map`
   - `src/stores/**` → `store`
   - `src/pages/dashboard/**` → `dashboard`
   - `apollo/**` → `api`
   - `src/18n.ts` / `locales/**` → `i18n`
   - `.github/**` / `.husky/**` → `ci`
2. 현재 git branch 이름 prefix
3. 대화 내 키워드
4. 위 모두 실패 → 사용자에게 질문

## Title 형식

`<type>(<scope>): <subject>`

- 소문자 시작, 100자 이내, 마침표 없음
- 한국어/영어 모두 OK

## Body 구조

### bug 이슈

- **Summary**: 증상 1줄
- **Steps to reproduce**: 재현 단계
- **Expected vs Actual**
- **Environment**: Node 버전, 브라우저, 화면 경로
- **Logs / Errors**: 에러 스택 (대화에 있으면 인용)
- **Severity**: Low / Medium / High / Critical

### feature 이슈

- **Problem / motivation**: 왜 필요한가
- **Proposed solution**: 어떻게 할 것인가
- **Acceptance criteria**: 검증 가능한 완료 조건 (체크박스)
- **Out of scope**: 이번에 안 하는 것
- **Risks**: 우려 사항

## 확인 후 생성

생성 전 title + body 미리보기 → 사용자 확인 → `gh issue create`.

```bash
gh issue create \
  --title "<title>" \
  --label "<bug|enhancement>" \
  --body "..."
```

생성 후 issue URL 표시.
