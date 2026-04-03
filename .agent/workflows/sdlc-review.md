---
description: Phase 6 — Security + quality + performance review with system visualization
argument-hint: [blank for local | PR number for PR review]
---

# P6: Code Review & Optimization

**Input**: $ARGUMENTS

## 1. Gather Changes

```bash
git diff --name-only HEAD
```

## 2. Security Review (FIRST)

- Secrets detection: grep for hardcoded passwords, API keys, tokens
- OWASP check: injection, broken auth, XSS, SSRF, missing validation
- Severity: CRITICAL (must fix) → HIGH (should fix) → MEDIUM → LOW

## 3. Quality Review

Read each changed file in full. Check:
- **Correctness**: logic errors, null handling, edge cases, race conditions
- **Type safety**: no `any`, proper generics, explicit return types
- **Patterns**: naming, file structure, error handling match codebase
- **Maintainability**: functions ≤50 lines, files ≤500 lines, nesting ≤4, no dead code

## 4. Performance Review

- Backend: N+1 queries, missing indexes, unbounded queries, memory leaks
- Frontend: unnecessary re-renders, large imports, missing lazy loading
- API: payload size, caching headers, pagination

## 5. Refactor & Optimize

Extract patterns, simplify logic, remove duplication. Validate after.

## 6. Visualize

Generate Mermaid diagrams: system architecture, data flow, component relationships.

## 7. Report

| Category | Severity | File:Line | Issue | Fix |
|----------|----------|-----------|-------|-----|

## Gate

> **Review complete.** {N} critical, {N} high, {N} medium, {N} low. Recommendation: APPROVE/CHANGES/BLOCK.
> - ✅ Approve → `/sdlc-test`
> - 🔧 Fix issues
> - ❌ Block → rework

**STOP. Wait for user approval.**
