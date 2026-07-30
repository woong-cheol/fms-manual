---
description: Analyze recurring failures or persistent issues from multiple perspectives (Contrarian + Simplifier + Researcher in parallel). Use when the same problem keeps coming back or a decision needs re-evaluation.
---

# /evolve — 다관점 분석으로 문제 진화

> 반복되는 실패나 잘못된 결정을 다각도로 분석해 다음 방향을 잡는다

## When to use

- 같은 버그가 반복해서 나타날 때
- 아키텍처 결정을 재검토해야 할 때
- `/unstuck` 으로도 해결이 안 될 때

## Instructions

### Subagent 병렬 분석

3개 관점을 동시에 실행:

```
Main Agent (Evolver)
  ├─ Subagent(Contrarian)  → "이 실패가 정말 문제인가? 전제가 틀렸나?"  (병렬)
  ├─ Subagent(Simplifier)  → "불필요한 복잡성이 원인은 아닌가?"         (병렬)
  └─ Subagent(Researcher)  → "유사 사례/증거가 있는가?"                 (병렬)
```

### Phase 1: Wonder ("우리가 아직 모르는 것은?")

현재 상황을 분석:

1. 무엇이 반복적으로 실패하는가?
2. 처음 결정 당시 가정이 지금도 유효한가?
3. 예상치 못한 부분이 발생했는가?
4. 놓친 요구사항이 있는가?

### Phase 2: Reflect ("피드백을 다음에 반영")

1. **패턴 식별**: 반복되는 실수 → 코드 컨벤션이나 구조 개선으로 해결 가능한가?
2. **결정 재평가**: 이 라이브러리/접근법이 여전히 최선인가?
3. **범위 조정**: 너무 크게 잡았거나 너무 좁게 잡은 게 있는가?

### Phase 3: Next Action

분석 결과에 따라:

- 코드 수정이 필요 → 구체적 파일:라인 제시
- 구조 변경이 필요 → `/unstuck` 으로 이어서 진행
- 결정 번복이 필요 → 대안과 트레이드오프 제시

## Output

```
═══ Evolution Report ══════════════════════════

Problem: {반복되는 문제}

Contrarian 관점: {전제 검토 결과}
Simplifier 관점: {제거 가능한 복잡성}
Researcher 관점: {근거/사례}

Root Cause: {진짜 원인}

Next Action:
  1. {구체적 액션}
  2. {구체적 액션}

→ 계속 막히면: /unstuck
```
