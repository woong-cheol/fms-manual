---
description: Fetch a PR (number or URL) and summarize — metadata, scope, file changes, CI checks, recent comments. Read-only.
argument-hint: '<pr-number-or-url>'
---

# /pr-read — PR 요약

> PR 번호나 URL을 받아 핵심 정보를 한눈에 보여준다. **읽기 전용** — 댓글/머지/체크아웃 하지 않는다.

## When to use

- 다른 사람 PR 검토 시작 전 컨텍스트 파악
- 본인 PR 상태 확인
- CI 실패 PR 원인 빠른 파악

## Argument

- `<n>` (숫자): `gh pr view N`
- URL: N 추출 후 동일
- 인자 없으면: 현재 브랜치 PR 자동 탐색

## Instructions

### Step 1: PR 메타데이터

```bash
gh pr view <N> --json number,title,state,author,baseRefName,headRefName,additions,deletions,changedFiles,labels,isDraft,mergeable,reviewDecision,body
```

### Step 2: 변경 파일 분류

```bash
gh pr diff <N> --name-only
```

파일을 영역별로 분류:

- **UI 컴포넌트**: `src/components/**`
- **페이지**: `src/pages/**` / `src/app/**`
- **상태**: `src/stores/**`
- **훅/유틸**: `src/hooks/**` / `src/utils/**`
- **GraphQL**: `apollo/**`
- **설정/인프라**: `.github/**` / `.husky/**` / `*.config.*`
- **정적 에셋**: `public/**`

### Step 3: CI 상태

```bash
gh pr checks <N>
```

각 체크 ✅/❌/⏳ 표시. 실패 있으면 첫 실패 메시지 한 줄.

### Step 4: 최근 활동

- 최근 코멘트 3개 (author + 첫 200자)
- 리뷰 요약 (APPROVED / CHANGES_REQUESTED)
- 대기 중인 reviewer

## 출력 형식

```
PR #N — feat(map): 차량 실시간 추적 기능 추가
  state: open · author: {author} · base: main ← head: {branch}
  +124/-8, 6 files · mergeable: MERGEABLE · review: APPROVED

Why: {PR 본문 요약}

Files (6):
  UI 컴포넌트: 2
  상태 관리:   1
  훅:          2
  설정:        1

CI: ✅ build · ✅ lint · ❌ test
  → first failure: {메시지}

Recent: 댓글 2, 리뷰 1 (APPROVED)

Next: /pr-review {N}  또는  gh run view ... --log-failed
```
