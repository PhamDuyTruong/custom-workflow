---
description: Quick reference for SDLC workflow commands
---

# SDLC Quick Reference

| Command | Phase | Gate |
|---------|-------|------|
| `/sdlc <desc>` | Full 7-phase | — |
| `/sdlc-analyze <desc>` | P1: Analyze + PRD | ✅ |
| `/sdlc-architect <PRD>` | P2: Architecture + Plan | ✅ |
| `/sdlc-design <plan>` | P3: Specs + Tasks | ✅ |
| `/sdlc-audit <plan>` | P4: Library + Security | ✅ |
| `/sdlc-implement <plan>` | P5: Code per task | — |
| `/sdlc-review` | P6: Review + Diagrams | ✅ |
| `/sdlc-test` | P7: Tests + QA | — |

## Shortcuts

- **Bug fix**: `/sdlc-implement` → `/sdlc-review` → `/sdlc-test`
- **Hotfix**: `/sdlc-implement` → `/sdlc-test`
- **Optimize prompt first**: `/prompt-master <desc>` → `/sdlc <optimized>`

## Artifacts

| Type | Path |
|------|------|
| PRDs | `.claude/PRPs/prds/{name}.prd.md` |
| Plans | `.claude/PRPs/plans/{name}.plan.md` |
| ADRs | `.claude/PRPs/adrs/ADR-{NNN}.md` |
| Specs | `openspec/changes/{name}/` or `.claude/PRPs/specs/{name}/` |
| Reports | `.claude/PRPs/reports/{name}-report.md` |
