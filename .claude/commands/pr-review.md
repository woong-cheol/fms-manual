---
description: Deep PR review against repo standards (CLAUDE.md, TypeScript, Next.js conventions, Conventional Commits, security). Outputs blockers/suggestions/nits.
argument-hint: '<pr-number-or-url> [--post]'
---

# /pr-review — PR 정밀 검토

> PR을 받아 이 repo의 룰셋으로 검토한다. 기본은 화면 출력, `--post`면 PR 코멘트로 게시.

## Arguments

- `<n>` 또는 PR URL — 필수
- `--post` (선택): 결과를 `gh pr comment`로 게시 (게시 전 확인)

## Preflight

```bash
gh pr view <N> --json title,state,mergeable
```

closed PR이면 read-only 분석.

## 리뷰 절차

### Phase 1: 컨텍스트 빌드

```bash
gh pr view <N> --json title,body,headRefName,changedFiles
gh pr diff <N>
```

### Phase 2: 정적 룰 검증

각 항목을 **BLOCKER / FAIL / NIT** 로 분류. `파일:라인` 명시 필수.

#### TypeScript

- `any` 타입 사용 → FAIL
- 타입 단언(`as X`) 남용 → FAIL
- 타입 가드 없는 유니온 처리 → FAIL

#### Next.js 컨벤션

- `<img>` 태그 직접 사용 (`next/image` 사용해야 함) → FAIL
- 페이지 컴포넌트에 비즈니스 로직 직접 작성 (hooks/utils 분리 권장) → FAIL
- `console.log` 잔존 → NIT

#### 보안

- 하드코딩된 API key / 서버 IP / 비밀번호 패턴 → BLOCKER
- `.env` 파일이 커밋에 포함 (`.env.example`만 허용) → BLOCKER
- `dangerouslySetInnerHTML` 무검증 사용 → BLOCKER

#### 상태 관리

- Recoil atom에 과도한 파생 로직 → FAIL (selector 분리 권장)
- 전역 상태를 props로 대체 가능한 경우 → NIT

#### Conventional Commits

- PR 제목이 `<type>(<scope>): <subject>` 형식이 아님 → FAIL
- breaking change인데 `!` 또는 `BREAKING CHANGE:` 없음 → BLOCKER

### Phase 3: 의미적 리뷰

정적 룰 통과 후 코드 품질 검토:

- **단순성**: 더 짧게 쓸 수 있는가?
- **컴포넌트 분리**: 한 컴포넌트가 너무 많은 역할을 하는가?
- **에러 처리**: API 실패/엣지 케이스가 처리됐는가?
- **성능**: 불필요한 리렌더링 유발 패턴이 있는가? (useCallback/useMemo 누락)
- **접근성**: 시맨틱 HTML, aria 속성 누락

### Phase 4: 출력

```
## Review summary — PR #N

Verdict: APPROVE / REQUEST_CHANGES / COMMENT

### Blockers (반드시 수정)
- file:line — 이유 / 수정 방향

### Suggestions (강한 권고)
- file:line — 이유 / 대안

### Nits (선택)
- file:line — 한 줄

### Approved areas
- ...
```

## 게시 (`--post`)

```bash
gh pr comment <N> --body-file /tmp/review.md
```

게시 전 사용자 확인 필수.
