---
description: Phase 1 — Analyze codebase + gather requirements → PRD. Auto-activates skills matching detected tech stack.
argument-hint: <feature description | requirement from BA/PM>
---

# P1: Codebase Analysis & Requirements

**Input**: $ARGUMENTS

## 1. Detect Context

```bash
ls -la src/ app/ lib/ 2>/dev/null || echo "NO_CODEBASE"
```
- Files found → Step 2 (codebase scan + skill activation)
- NO_CODEBASE → Skip to Step 3

## 2. Codebase Scan & Skill Activation

### 2.1 Tech Stack Detection

Detect stack by scanning config files:

```bash
# Language & runtime
cat package.json 2>/dev/null && echo "::NODE"
cat tsconfig.json 2>/dev/null && echo "::TYPESCRIPT"
cat requirements.txt setup.py pyproject.toml 2>/dev/null && echo "::PYTHON"
cat go.mod 2>/dev/null && echo "::GO"
cat Cargo.toml 2>/dev/null && echo "::RUST"
cat build.gradle* pom.xml 2>/dev/null && echo "::JAVA_KOTLIN"
cat pubspec.yaml 2>/dev/null && echo "::FLUTTER"
cat Package.swift 2>/dev/null && echo "::SWIFT"
cat Gemfile 2>/dev/null && echo "::RUBY"
cat composer.json 2>/dev/null && echo "::PHP"
cat CMakeLists.txt Makefile 2>/dev/null && echo "::CPP"

# Framework detection (from package.json or imports)
grep -l "next" package.json 2>/dev/null && echo "::NEXTJS"
grep -l "react" package.json 2>/dev/null && echo "::REACT"
grep -l "express\|hono\|fastify\|koa" package.json 2>/dev/null && echo "::NODE_BACKEND"
grep -l "prisma" package.json 2>/dev/null && echo "::PRISMA"
grep -l "django\|Django" requirements.txt setup.py 2>/dev/null && echo "::DJANGO"
grep -l "laravel\|Laravel" composer.json 2>/dev/null && echo "::LARAVEL"
grep -l "spring\|Spring" build.gradle* pom.xml 2>/dev/null && echo "::SPRINGBOOT"
grep -l "ktor\|Ktor" build.gradle* 2>/dev/null && echo "::KTOR"
ls *.proto 2>/dev/null && echo "::GRPC"

# Database detection
grep -rl "postgres\|postgresql" . --include="*.json" --include="*.yml" --include="*.env*" -l 2>/dev/null | head -1 && echo "::POSTGRES"
grep -rl "mongo" . --include="*.json" --include="*.yml" -l 2>/dev/null | head -1 && echo "::MONGO"
grep -rl "redis" . --include="*.json" --include="*.yml" -l 2>/dev/null | head -1 && echo "::REDIS"

# Infra detection
ls Dockerfile docker-compose* 2>/dev/null && echo "::DOCKER"
ls .github/workflows/*.yml 2>/dev/null && echo "::GITHUB_ACTIONS"
ls vercel.json next.config* 2>/dev/null && echo "::VERCEL"
```

### 2.2 Activate Skills by Detected Stack

Based on detection results, activate the matching skills **for this session only**:

| Detected | ECC Agent Skills | ECC Skills | Antigravity Skills |
|----------|-----------------|------------|-------------------|
| **TypeScript** | `typescript-reviewer` | `coding-standards` | `typescript-pro` |
| **Node.js** | `build-error-resolver` | `backend-patterns` | `nodejs-best-practices` |
| **Next.js** | — | `nextjs-turbopack` | `nextjs-best-practices` |
| **React** | — | `frontend-patterns` | `react-patterns`, `react-component-performance` |
| **Express/Hono** | — | `api-design` | `hono`, `api-patterns` |
| **Prisma** | `database-reviewer` | `database-migrations` | `prisma-expert` |
| **PostgreSQL** | `database-reviewer` | `postgres-patterns` | `postgresql`, `database-optimizer` |
| **Python** | `python-reviewer` | `python-patterns`, `python-testing` | — |
| **Django** | `python-reviewer` | `django-patterns`, `django-tdd`, `django-security` | — |
| **Go** | `go-reviewer`, `go-build-resolver` | `golang-patterns`, `golang-testing` | — |
| **Rust** | `rust-reviewer`, `rust-build-resolver` | `rust-patterns`, `rust-testing` | — |
| **Java** | `java-reviewer`, `java-build-resolver` | `java-coding-standards`, `jpa-patterns` | — |
| **Kotlin** | `kotlin-reviewer`, `kotlin-build-resolver` | `kotlin-patterns`, `kotlin-testing`, `kotlin-coroutines-flows` | — |
| **Spring Boot** | `java-reviewer` | `springboot-patterns`, `springboot-tdd`, `springboot-security` | — |
| **Ktor** | `kotlin-reviewer` | `kotlin-ktor-patterns` | — |
| **Flutter** | `flutter-reviewer` | `flutter-dart-code-review` | — |
| **Swift** | — | `swift-concurrency-6-2`, `swift-protocol-di-testing` | — |
| **Laravel** | — | `laravel-patterns`, `laravel-tdd`, `laravel-security` | — |
| **C++** | `cpp-reviewer`, `cpp-build-resolver` | `cpp-coding-standards`, `cpp-testing` | — |
| **Docker** | — | `docker-patterns` | `docker-expert` |
| **GitHub Actions** | — | — | `github-actions-templates`, `github` |
| **Vercel** | — | — | `vercel-deployment` |
| **GraphQL** | — | — | `graphql` |

**Always activated** (all projects): `architect`, `planner`, `code-reviewer`, `security-reviewer`, `tdd-guide`, `performance-optimizer` (ECC agents) + `coding-standards`, `codebase-onboarding`, `repo-scan` (ECC skills).

**Log activated skills**:
```
[skills] Detected: TypeScript + Next.js + Prisma + PostgreSQL + Docker
[skills] Activated: typescript-reviewer, nextjs-turbopack, prisma-expert, postgresql, database-reviewer, docker-expert + base skills
```

### 2.3 Convention Extraction

Use activated skills to scan and extract into `.claude/analysis/codebase-profile.md`:

| Category | Extract |
|----------|---------|
| Stack | Runtime, framework, language, DB, ORM, test framework |
| Architecture | Pattern (MVC/Clean/Feature), layers, data flow |
| Naming | File/function/class conventions |
| Imports | Absolute/relative, barrel exports |
| Error Handling | Error classes, try/catch patterns |
| Types | Interface vs Type, naming, location |
| Tests | Framework, naming, setup/teardown |

Trace data flow: entry → middleware → route → controller → service → repo → DB.

**Include in profile**: list of activated skills for subsequent phases to reuse.

## 3. Requirements Gathering

**3.1** Restate requirement → wait for confirmation.

**3.2** Ask foundation questions (all at once):
1. **Who** has this problem?
2. **What** pain are they facing?
3. **Why** can't they solve it today?
4. **Why now?**
5. **How** will we measure success?

**3.3** Research competitors, existing solutions, codebase patterns.

**3.4** Scope questions:
1. MVP definition
2. Must-have vs nice-to-have
3. Out of scope
4. Constraints
5. Open questions

## 4. Generate PRD

```bash
mkdir -p .claude/PRPs/prds
```
Write to `.claude/PRPs/prds/{kebab-case-name}.prd.md` (use `/prp-prd` template).

**Include in PRD footer**: detected tech stack + activated skills for downstream phases.

## Gate

> **Phase 1 Complete.** PRD: `.claude/PRPs/prds/{name}.prd.md`
> **Stack**: {detected stack}
> **Skills activated**: {list}
> - ✅ Approve → `/sdlc-architect`
> - ✏️ Modify → update
> - ❌ Stop

**STOP. Wait for user approval.**
