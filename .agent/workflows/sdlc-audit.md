---
description: Phase 4 — Library audit (version, license, CVEs, docs) + security pre-check
argument-hint: <plan path>
---

# P4: Pre-Implementation Audit

**Input**: $ARGUMENTS — Extract all dependencies from plan + tasks.

## 1. Library Audit

For EVERY new library:

```bash
npm view {lib} version description license homepage
```

Report table:

| Library | Version | License | CVEs | Official Docs | Purpose |
|---------|---------|---------|------|---------------|---------|

Check: license compatibility (MIT/Apache ✅, GPL ⚠️, AGPL ❌ for SaaS), known CVEs (`npm audit`), bundle size impact.

## 2. Security Assessment

OWASP Top 10 pre-check against design:

| # | Category | Risk | Mitigation |
|---|----------|------|------------|
| A01-A10 | ... | H/M/L | ... |

Security plan: auth method, authz model, input validation (Zod/Joi), data encryption, secrets management, rate limiting, CORS, CSP headers.

## 3. Compatibility

Version matrix (Node.js, TS, React vs library requirements). Peer dependency conflicts. Bundle size delta.

## Gate

> **Phase 4 Complete.** {N} deps audited, {N} CVEs, OWASP {X}/10 covered.
> - ✅ Approve → `/sdlc-implement`
> - ⚠️ Concerns → discuss alternatives
> - ❌ Block → fix first

**STOP. Wait for user approval.**
