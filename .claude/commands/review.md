---
description: Quick code quality check — runs TypeScript build, ESLint, and reviews recent changes for common issues. Use between feature implementations to catch problems early.
argument-hint: '[optional: file or directory to focus on]'
---

# /review — 코드 빠른 점검

> TypeScript 타입체크 + ESLint + 코드 품질 검토를 빠르게 실행한다.

## When to use

- 기능 구현 완료 후 PR 올리기 전
- 변경이 많아 놓친 게 있을 것 같을 때
- `/pr-review` 전 사전 자가 점검

## Instructions

### Step 1: Mechanical 검사

```bash
# TypeScript 타입 오류 확인
npx tsc --noEmit

# ESLint 검사
npm run lint
```

인자가 있으면 해당 파일/디렉토리에 한정:

- `/review src/components/map` → 해당 경로만 lint

### Step 2: 최근 변경 코드 리뷰

```bash
git diff HEAD --stat
git diff HEAD
```

변경된 파일을 읽고 다음을 검사:

- `any` 타입 사용 여부
- `console.log` 잔존 여부
- 하드코딩된 URL/IP/key 여부
- 컴포넌트에 비즈니스 로직이 직접 들어갔는지 (hooks/utils로 분리 권장)
- `img` 태그 직접 사용 여부 (`next/image` 권장)
- Recoil atom에 파생 로직이 과도하게 들어갔는지

### Step 3: 결과 요약

```
═══ /review ═══════════════════════════════════

Mechanical:
  tsc --noEmit:  PASS | FAIL ({n} errors)
  eslint:        PASS | FAIL ({n} errors)

Code Issues:
  any 사용:      {n}곳
  console.log:   {n}곳
  하드코딩 값:   {n}곳
  기타:          ...

Verdict: PASS | WARN | FAIL
  → PASS: PR 올려도 됩니다
  → WARN: 확인 후 진행 가능
  → FAIL: 수정 필요

Time: {elapsed}
═══════════════════════════════════════════════
```
