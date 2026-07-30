# 리팩토링 계획 — main-front

| 항목 | 내용 |
|------|------|
| 문서 타입 | 작업 계획 |
| 작성일 | 2026-07-03 |
| 작성자 | 최웅철M |
| 버전 | v0.1 |
| 상태 | 진행 전 |

---

## 개요

전체 소스 파일 602개 규모의 프로젝트 품질 개선을 위한 단계별 리팩토링 계획.
리스크가 낮은 클린업부터 시작하여 의존성 업그레이드 순으로 진행한다.

---

## Phase 1 — 클린업 (낮은 위험)

### 1-1. 테마/스타일 통합
- [x] `palettes.ts`, `new-palettes.ts`, `theme/index.tsx` 사용처 확인 후 제거
- [x] `newTheme.ts` 단일 파일로 일원화
- [x] `@mui/styles` (MUI v4 레거시) 의존성 제거

### 1-2. `utils.ts` 분리 (현재 449줄)
- [x] `mapUtils.ts` — 지도/좌표 관련 유틸
- [x] `dateUtils.ts` — 날짜/시간 관련 유틸
- [x] `colorUtils.ts` — 색상 관련 유틸
- [x] 나머지 공통 유틸 정리 (utils.ts 배럴 re-export 유지)

### 1-3. GQL 쿼리 정리
- [x] Fragment 도입으로 중복 필드 선언 제거
- [x] `FetchUserBySelf` company 블록에 `allowedMainMenuTypes` 추가
- [x] `FETCH_USERS_BY_COMPANY` 중복 쿼리명/본문 수정 및 변수 버그 수정
- [x] `FETCH_ALL_ROLES` / `FETCH_ROLES_BY_COMPANY` RoleFields Fragment 도입
- [x] `CREATE_MAP` / `UPDATE_MAP` MapFields Fragment 도입

---

## Phase 2 — 구조 개선 (중간 위험)

### 2-1. Recoil 스토어 통합
- [ ] 현재 30개 이상 소형 store → 도메인별 그룹핑
  - `mapAtoms` — viewport, mapBaseLayer, mapFeature 등
  - `vehicleAtoms` — companyVehicle, selectedVehicle 등
  - `uiAtoms` — leftSidebar, overlay, screen 등

### 2-2. 대형 컴포넌트 분해
- [ ] `LeftSidebar.tsx` (421줄) — 역할 단위로 분리
- [ ] `newTheme.ts` (520줄) — palette / component overrides / type 선언 분리

### 2-3. `react-table v7` → TanStack Table v8
- [ ] API 마이그레이션
- [ ] 타입 개선 및 성능 향상 확인

---

## Phase 3 — 의존성 업그레이드 (높은 위험)

### 3-1. `react-beautiful-dnd` → `dnd-kit`
- [ ] 아카이브 상태 라이브러리 교체 (유지보수 중단)
- [ ] 사용처 파악 및 마이그레이션

### 3-2. Recoil → Jotai 마이그레이션 검토
- [ ] Recoil 유지보수 중단 현황 재확인
- [ ] Jotai 호환성 및 마이그레이션 비용 평가
- [ ] 단계적 교체 전략 수립

### 3-3. Next.js 13.0.6 → 최신 업그레이드
- [ ] 버전별 Breaking Change 확인
- [ ] Pages Router 유지 또는 App Router 전환 결정
- [ ] 단계적 업그레이드 진행

---

## 우선순위 요약

| 단계 | 작업 | 리스크 | 효과 |
|------|------|--------|------|
| 1-1 | 테마 통합 | 낮음 | 높음 |
| 1-2 | utils 분리 | 낮음 | 중간 |
| 1-3 | GQL 정리 | 낮음 | 중간 |
| 2-1 | 스토어 통합 | 중간 | 높음 |
| 2-2 | 컴포넌트 분해 | 중간 | 중간 |
| 2-3 | react-table 업그레이드 | 중간 | 높음 |
| 3-1 | dnd-kit 교체 | 높음 | 중간 |
| 3-2 | Recoil → Jotai | 높음 | 높음 |
| 3-3 | Next.js 업그레이드 | 매우 높음 | 높음 |

---

## 진행 현황

| 항목 | 상태 | 완료일 |
|------|------|--------|
| — | — | — |
