---
description: Create a PR from the current branch — runs preflight checks, auto-fills title and body (Why/What/Risk/Verified/ReviewFocus), opens via gh.
argument-hint: '[optional: PR title in Conventional Commit form]'
---

# /pr-create — 현재 브랜치를 PR로 열기

> 변경 내용을 분석해 PR 제목과 본문을 자동으로 작성하고 gh로 생성한다.

## Preflight (모두 통과해야 진행)

1. 현재 브랜치가 `main`/`dev`가 **아닌지** 확인. 맞으면 거부.
2. `git status --porcelain` clean 여부. 미커밋 변경 있으면 중단.
3. `git log origin/main..HEAD` — 커밋 최소 1개 있는지.
4. 게이트 실행:
   ```bash
   npx tsc --noEmit
   npm run lint
   ```

## PR 본문 자동 작성

### Why

- 브랜치의 첫/최근 커밋 메시지 body에서 의도 추출
- `Closes #NN` 있으면 그대로 포함

### What changed

- `git diff origin/main..HEAD --stat` 주요 파일 5개 의도 요약
- diff 묘사 X, "X 기능 추가 / Y 버그 수정 / Z 리팩토링" 위주

### Scope

변경 파일 경로 기반 자동 분류:

- `src/components/**` → UI 컴포넌트
- `src/stores/**` → 상태 관리 (Recoil)
- `src/hooks/**` → 커스텀 훅
- `src/utils/**` → 유틸리티
- `src/pages/**` / `src/app/**` → 페이지
- `apollo/**` / `codegen.ts` → GraphQL
- `.github/**` / `.husky/**` / `.harness/**` → CI/인프라
- `public/**` → 정적 에셋

### Risk

- 하드코딩 값 변경 (서버 IP, URL) → 영향 범위 명시
- 새 의존성 추가 여부 (`package.json` diff)
- breaking change 가능성

### How I verified

- `tsc --noEmit` 결과
- `npm run lint` 결과
- 브라우저 수동 확인 여부

### Review focus

- 변경 라인 50줄 이상 파일
- 새로 추가된 컴포넌트/훅
- 비즈니스 로직이 있는 파일

## 실제 호출

```bash
git push -u origin <current-branch>

gh pr create \
  --base main \
  --title "<title>" \
  --body "..."
```

생성 후 PR URL 표시. `--draft` 인자 받으면 draft PR로 생성.
생성 직후 `/pr-review <N>` 셀프 리뷰 제안.
