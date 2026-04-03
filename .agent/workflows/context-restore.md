---
description: Restore project context at start of a new conversation or change request. Run this FIRST in every new session to avoid hallucination.
---

# Context Restore — Start of Session

**When to use**: At the START of every new conversation, BEFORE doing any work.

**Input**: `$ARGUMENTS` — project name or path to context/spec/plan file

> Inspired by [Trellis workflow](https://github.com/mindfold-ai/trellis) — "Read before write, follow standards, record promptly, capture learnings"

## Steps

### 1. Load Context Documents (MANDATORY)

Read these files in order to rebuild mental model:

```
# Priority 1: Current state (most important — what's happening NOW)
.claude/PRPs/context/{project-name}.context.md

# Priority 2: Session history (WHY decisions were made)
.claude/PRPs/context/{project-name}.journal.md
→ Read last 2-3 entries only (most recent sessions)

# Priority 3: What we're building
.claude/PRPs/prds/{project-name}.prd.md

# Priority 4: How we're building it
.claude/PRPs/plans/{project-name}.plan.md

# Priority 5: Design decisions & known gotchas
.claude/PRPs/adrs/

# Priority 6: Detailed specs (including Lessons Learned sections)
.claude/PRPs/specs/
```

### 2. Verify Codebase State

```bash
# Check current branch and uncommitted changes
git status
git log --oneline -5

# Check installed dependencies match plan
cat package.json | head -30

# Check for any build errors
npx tsc --noEmit 2>&1 | head -20
```

### 3. Cross-Reference Context vs Code

Compare what the context file says vs actual code:
- Are the files mentioned in "Files Modified" actually there?
- Does `package.json` match the tech stack in context?
- Are the "Known Issues" still present or resolved?
- Do "Key Patterns Established" match the current code?

### 4. Load Lessons Learned

> Trellis principle: Previous sessions' gotchas prevent repeated mistakes.

Check for "Lessons Learned" sections in:
- Journal entries (recent bugs fixed + patterns discovered)
- Spec files (accumulated gotchas per framework/tool)
- ADRs (architecture decisions and their rationale)

### 5. Generate Resumption Summary

Output to user:

```
## 🔄 Context Restored: {Project Name}

**Phase**: P{N} — {phase name}
**Last session**: {summary from journal}
**Completed**: {X}/{Total} tasks
**Sessions recorded**: {N} journal entries

### Ready to resume:
1. {next task from plan}
2. {next task}

### Blockers from last session:
- {any unresolved issues}

### Key patterns to follow:
- {pattern 1 from Lessons Learned}
- {pattern 2}

### Known gotchas (from journal):
- {gotcha 1}
- {gotcha 2}

### Shall I proceed with task {next}?
```

## Rules

1. **NEVER assume** — if context file is missing, ask user what they want to do
2. **ALWAYS verify** — cross-reference context docs with actual code
3. **FLAG conflicts** — if code doesn't match docs, report to user before proceeding
4. **READ, don't scan** — actually load and process the full content of each document
5. **RESPECT patterns** — follow "Key Patterns Established" from context file, don't invent new ones
6. **LEARN from journal** — check Lessons Learned before implementing anything similar to past work
