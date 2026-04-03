---
description: Phase 5 — Execute implementation per-task with continuous validation. Includes framework knowledge verification.
argument-hint: <plan path>
---

# P5: Implementation

**Input**: $ARGUMENTS — Read approved plan + tasks.

## 0. Pre-Implementation: Framework Knowledge Check

**MANDATORY** before writing any code. For each framework/library in the stack:

### 0.1 Verify Current Version Constraints

```bash
# Check exact installed versions
npm ls --depth=0 2>/dev/null || cat package.json
```

Research the **exact version's** breaking changes, migration notes, and known gotchas:

| Framework | Common Pitfalls to Verify |
|-----------|--------------------------|
| **React 19** | No `useEffect(() => setState())` for mount detection (cascading renders). Use `useSyncExternalStore` or `next/dynamic`. |
| **Next.js 15+** | App Router: pages are Server Components by default. `"use client"` required for hooks/state/browser APIs. `next/dynamic` with `ssr:false` needs Client Component. `output:"export"` disables API routes. |
| **TypeScript 5** | `satisfies` operator, `const` type params, `using` declarations. No `any` — use `unknown`. |
| **Mantine 7** | `MantineProvider` required at root. `createTheme()` for customization. CSS Modules, not Tailwind. |
| **Dexie 4** | `useLiveQuery` in client components only. `EntityTable` generic. Race conditions on double-mount → use singleton pattern. |
| **BlockNote** | Peer deps matter (check Mantine version). `useCreateBlockNote` hook. Import CSS in correct order. |

### 0.2 Search for Latest Gotchas

For EACH major library, run:
```
Search: "{library} {version} known issues gotchas breaking changes 2025 2026"
```

Document findings in a quick checklist:
```
[✓] React 19: No setState in useEffect for mounting
[✓] Next.js 16: "use client" for all interactive components
[✓] Dexie 4: Singleton seed to prevent race condition
[✓] BlockNote: Pin Mantine 7.x for compatibility
```

### 0.3 Verify Patterns Before Writing

Before implementing ANY component, verify the pattern against official docs:
- SSR vs CSR rendering boundary → where to put `"use client"`
- Data fetching pattern → client-side with hooks (not `getServerSideProps`)
- State hydration → avoid hydration mismatch (no `useState(Date.now())`)
- Import order → CSS imports before component imports

**Only proceed to Step 1 after checklist is complete.**

## 1. Setup

Detect package manager (`bun.lockb`→bun, `pnpm-lock.yaml`→pnpm, `yarn.lock`→yarn, `package-lock.json`→npm). Note available scripts (type-check, lint, test, build).

Git: if on main with clean tree → `git checkout -b feat/{name}`. Pull latest.

Install approved new dependencies.

## 2. Per-Task Loop

For each task (in dependency order):

1. **Check pattern** — verify intended approach against Step 0 checklist
2. **Read** MIRROR pattern from plan
3. **Implement** following patterns exactly
4. **Validate immediately** after every file:
   ```bash
   npx tsc --noEmit    # type-check
   npx eslint . --fix  # lint
   ```
5. **Runtime check** — open browser/run test to verify no runtime errors
6. **Fix errors** before next file — never accumulate broken state
7. **Log**: `[done] Task {ID}: {name}`

## 3. Implementation Order

**Backend**: DB migrations → Models/Types → Repository → Service → Controller/Routes → Middleware

**Frontend**: Types/Schemas → API client/hooks → Shared components → Feature components → Pages → Integration

## 4. Deviations

If deviating from plan: note WHAT + WHY + IMPACT. Report in Phase 6.

## Output

Progress log with task status, validation results, knowledge-check findings, deviations list. → Auto-proceed to `/sdlc-review`.
