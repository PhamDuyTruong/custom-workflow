---
description: Phase 7 — Unit + integration + E2E tests, coverage check, self-test guide
argument-hint: [blank for current changes | specific test area]
---

# P7: Testing & QA

**Input**: $ARGUMENTS

## 1. Unit Tests

Write tests for every new function/method. Structure: describe → it (Arrange/Act/Assert).

Coverage targets: 100% auth/payments/critical, 80%+ overall.

Edge cases: empty/null, max size, invalid types, boundaries, concurrent access, network failure, permission denied.

## 2. Integration Tests

API endpoints: test success (2xx), auth failure (401), validation error (400), not found (404).
DB operations: CRUD, transactions, constraint violations.

## 3. E2E Tests (if frontend)

Playwright for critical user flows. Skip if backend-only.

## 4. Validation Suite

```bash
npx tsc --noEmit          # zero type errors
npx eslint . --ext .ts    # zero lint errors
npm test -- --coverage    # all pass, ≥80%
npm run build             # success
```

Fix implementation (not tests) if failures occur.

## 5. Self-Test Guide

Provide user: how to start dev server, access feature, what to verify manually.

## Output

```
Tests: {N} unit, {N} integration, {N} e2e — all passing
Coverage: {X}%
Validation: type-check ✅ lint ✅ build ✅
```

Post-phase: `git commit -m "feat({scope}): {desc}"` → optional `gh pr create`.
