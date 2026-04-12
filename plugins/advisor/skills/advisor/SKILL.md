---
name: advisor
description: "Use when starting a project, planning a feature, or unsure which skills to use. Scans your repo and recommends the right skills for your situation with plain-language explanations."
metadata:
  author: zachd
  version: "2.0.0"
---

# Skill Advisor

## Overview

Help developers figure out which Claude Code skills to use. Scan the repo, size the task, and recommend the right skills at the right level of process. Small tasks get framework guidance and nothing else. Big tasks get the full pipeline. The advisor is the gatekeeper — it decides how much ceremony a task needs.

<HARD-GATE>
Do NOT write any files. This skill produces chat output ONLY.
Do NOT invoke other skills. This skill RECOMMENDS skills — it does not run them.
Do NOT answer the user's question or start working on their task. Your ONLY job is to recommend skills.
Do NOT provide a gameplan, implementation, code, or solution. Just recommend skills using the template below.
Even if the user describes a specific task alongside invoking this skill, treat it as context for your recommendations — not as a request to do the work.
</HARD-GATE>

## Phase 1: Fast First Pass

When the user invokes `/advisor`, follow these steps in order:

### Step 1: Ask what they're working on

Ask a single question:

> What are you trying to build or work on?

Wait for their answer before proceeding.

### Step 2: Scan the repo for context

Silently gather information about the project. Do not narrate what you're scanning — just do it. If no project files are found, note "No project detected" in the stack line and focus recommendations on the user's description alone.

**Read these files if they exist:**
- `CLAUDE.md` — project-specific context, conventions, known issues
- `package.json` — dependencies reveal frameworks and libraries
- `requirements.txt`, `pyproject.toml`, `setup.py`, `setup.cfg` — Python dependencies
- `go.mod` — Go project
- `Cargo.toml` — Rust project
- `composer.json` — PHP dependencies
- `Gemfile` — Ruby dependencies
- `pom.xml`, `build.gradle`, `build.gradle.kts` — Java/Kotlin dependencies
- `pubspec.yaml` — Flutter/Dart
- `.env.example` or `.env.local` — what services are integrated

**Check directory structure:**
- `app/` — likely Next.js App Router or Rails
- `src/` — standard source directory
- `pages/` — Next.js Pages Router or similar
- `tests/`, `__tests__/`, `spec/` — test directories
- `wp-content/` — WordPress
- `.github/workflows/` — GitHub Actions CI/CD
- `Dockerfile`, `docker-compose.yml` — containerized deployment
- `.tf` files — Terraform
- `k8s/`, Helm charts — Kubernetes
- `.xcodeproj`, `Package.swift` — iOS/macOS Swift
- `.csproj` — C#/.NET

**Check config files:**
- `tsconfig.json` — TypeScript
- `tailwind.config.*` — Tailwind CSS
- `vercel.json` — Vercel deployment
- `.gitignore` — verify it exists and covers secrets

### Step 3: Size the task

This is the most important step. Based on the user's description, classify the task into one of three tiers:

**Quick** — Do it directly, maybe with a framework skill for reference.
- Bug fixes, typos, config changes
- Single-file changes with clear intent
- Adding a field, tweaking styles, small refactors
- "Fix X", "Change Y to Z", "Update the config"
- Anything you could explain in one sentence

**Build** — Use `/pair` for controlled step-by-step implementation, plus framework skills.
- New component, new API route, new page
- Feature that touches 2-5 files
- Something with a clear scope but enough complexity to benefit from human review at each step
- "Add a settings page", "Create an API endpoint for X", "Build a card component"

**Architect** — Use the full pipeline: brainstorming → writing-plans → execution.
- Multi-system features (frontend + backend + database + auth)
- New projects or major rewrites
- Features that need design decisions before coding
- "Build a notification system", "Add real-time chat", "Redesign the auth flow"
- Anything where you'd want to think through the approach before touching code

**When in doubt, size DOWN.** Most tasks are smaller than they feel. A Quick task done well beats a Build task buried in process.

### Step 4: Match framework skills

Based on the detected stack, identify which framework-specific skills are relevant. These are always recommended regardless of task size — they're reference material, not process.

Pick only what matches. If it's a Next.js + TypeScript + Tailwind project, recommend `nextjs-developer`, `typescript-pro`, and `react-expert`. Don't recommend `python-pro` just because it exists.

### Step 5: Output recommendations

Use the format for the detected tier:

---

#### For Quick tasks:

```
## Advisor: Quick Task

**Task:** [what they described]
**Stack:** [detected stack]
**Tier:** Quick — just do it

### Framework Guidance
- **skill-name** — [why it's relevant to THIS task]

### Go
No special workflow needed. Just describe what you want and Claude will do it.
If you want framework-specific guidance loaded, say: "Use [skill-name] for reference while you [task]."

### Watch Out
- [Only if there's a genuine blind spot — otherwise skip this section entirely]
```

---

#### For Build tasks:

```
## Advisor: Build Task

**Task:** [what they described]
**Stack:** [detected stack]
**Tier:** Build — step-by-step with review

### Framework Guidance
- **skill-name** — [why it's relevant to THIS task]

### Workflow
Use `/pair` to build this step by step. Claude proposes each change, you approve before it's written.

1. `/pair [task description]` — build it with approval at each step
2. When done, review and commit

### Watch Out
- [Blind spots relevant to this specific task]
```

---

#### For Architect tasks:

```
## Advisor: Architect Task

**Task:** [what they described]
**Stack:** [detected stack]
**Tier:** Architect — think first, then build

### Framework Guidance
- **skill-name** — [why it's relevant to THIS task]

### Process Skills
- **brainstorming** — [why this task needs design exploration first]
- **writing-plans** — [why this task benefits from a structured plan]
- [Other process skills ONLY if genuinely needed — don't list them all]

### Workflow
1. **brainstorming** → explore the design space, nail down what you're building
2. **writing-plans** → create the implementation plan
3. **[framework skill]** → use as reference during implementation
4. **verification-before-completion** → sanity check before shipping

> **To start:** "Use brainstorming to design [task], then writing-plans to create the implementation plan."

### Watch Out
- [Blind spots, architectural concerns, security considerations]
```

---

### Step 6: Make recommendations contextual

Each skill recommendation MUST include a contextual explanation — not a generic description of the skill, but why it matters for what THIS user is building.

Good: "**nextjs-developer** — Your app uses the Next.js App Router, and adding this feature will likely involve server components and route handlers"

Bad: "**nextjs-developer** — Helps with Next.js development"

Good: "**secure-code-guardian** — Since this feature handles user profile data with Supabase RLS, it's worth checking your auth and data access patterns"

Bad: "**secure-code-guardian** — Checks security patterns"

### Step 7: Check skill availability

Before outputting recommendations, check which skills the user actually has installed. The system reminder at the start of every session lists all available skills. Cross-reference your recommendations against that list.

- If a recommended skill IS available — no extra note needed
- If a recommended skill is NOT available — append a note: *(not installed — available in the [plugin-name] plugin)*

**Plugin mapping for common skills:**
- `superpowers` plugin: brainstorming, writing-plans, executing-plans, subagent-driven-development, dispatching-parallel-agents, systematic-debugging, verification-before-completion, finishing-a-development-branch, requesting-code-review, receiving-code-review, test-driven-development, using-git-worktrees
- `fullstack-dev-skills` plugin: All framework/language skills (nextjs-developer, react-expert, python-pro, typescript-pro, etc.), architecture-designer, api-designer, the-fool, database-optimizer, etc.
- `document-skills` plugin: pdf, docx, xlsx, pptx, frontend-design, canvas-design, mcp-builder, skill-creator
- `code-review` plugin: code-review
- `commit-commands` plugin: commit, commit-push-pr, clean_gone
- `code-simplifier` plugin: simplify
- `claude-md-management` plugin: claude-md-improver, revise-claude-md
- `feature-dev` plugin: feature-dev
- Custom skills (zachd): advisor, retro, setup, pair, checkpoint

## Phase 2: Refinement

If the user says yes to going deeper or disagrees with the tier:

### Tier override

If the user says "this is bigger/smaller than you think," respect that immediately. Re-tier and re-recommend. The user knows their codebase better than a file scan does.

### Ask 2-3 clarifying questions

Ask targeted, context-aware questions in plain language. Do not ask about things that are already obvious from the repo scan or user's description.

- "Are you building something new or changing something that already exists?"
- "What parts does this affect — how things look, how data is stored, backend logic, or a mix?"
- "Does this involve user accounts, personal info, or login?"
- "Does this feature use AI to generate or analyze anything?"
- "Is this going to production soon, or is it more experimental?"

Pick only the questions whose answers would actually change your tier or recommendations.

### Output refined recommendations

After the user answers, output updated recommendations in the same template format. Note what changed and why if it's not obvious.

## Framework Skill Taxonomy

This is the knowledge base for matching framework skills to stacks.

### Frontend

| Skill | When to recommend |
|-------|-------------------|
| nextjs-developer | `package.json` has `"next"` |
| react-expert | `package.json` has `"react"` |
| vue-expert | `package.json` has `"vue"` |
| angular-architect | `package.json` has `"@angular/core"` |
| frontend-design | Task involves visual design or UI work |
| react-native-expert | `package.json` has `"react-native"` |
| flutter-expert | `pubspec.yaml` exists |

### Backend

| Skill | When to recommend |
|-------|-------------------|
| python-pro | Python project detected |
| fastapi-expert | Dependencies include `"fastapi"` |
| django-expert | Dependencies include `"django"` |
| golang-pro | `go.mod` exists |
| rust-engineer | `Cargo.toml` exists |
| java-architect | `pom.xml` or `build.gradle` exists |
| csharp-developer | `.csproj` files exist |
| kotlin-specialist | `build.gradle.kts` with kotlin |
| php-pro | `composer.json` exists |
| rails-expert | `Gemfile` has `"rails"` |
| laravel-specialist | `composer.json` has `"laravel"` |
| nestjs-expert | `package.json` has `"@nestjs/core"` |
| dotnet-core-expert | `.csproj` with `net8` |
| spring-boot-engineer | `pom.xml` with `spring-boot` |
| swift-expert | `.xcodeproj` or `Package.swift` exists |
| wordpress-pro | `wp-content` directory |

### TypeScript / JavaScript

| Skill | When to recommend |
|-------|-------------------|
| typescript-pro | `tsconfig.json` exists |
| javascript-pro | `.js` files without TypeScript |

### Data & AI

| Skill | When to recommend |
|-------|-------------------|
| prompt-engineer | Task involves AI/LLM features |
| rag-architect | Task involves knowledge retrieval + AI |
| ml-pipeline | Task involves model training |
| claude-api | Code imports `anthropic` SDK |
| pandas-pro | Using pandas for data processing |

### Database

| Skill | When to recommend |
|-------|-------------------|
| postgres-pro | Using PostgreSQL or Supabase |
| sql-pro | Any SQL database |
| database-optimizer | Performance issues with database |

### API Design

| Skill | When to recommend |
|-------|-------------------|
| api-designer | Building new APIs |
| graphql-architect | Using GraphQL |
| websocket-engineer | Building real-time features |
| mcp-developer | Building MCP tool integrations |

### Infrastructure

| Skill | When to recommend |
|-------|-------------------|
| devops-engineer | `Dockerfile`, `.github/workflows/`, or CI config present |
| terraform-engineer | `.tf` files present |
| kubernetes-specialist | K8s manifests or Helm charts present |
| cloud-architect | Cloud infrastructure decisions |
| monitoring-expert | Setting up monitoring or debugging production issues |

### Specialized

| Skill | When to recommend |
|-------|-------------------|
| cli-developer | Task is a command-line tool |
| embedded-systems | Microcontroller or embedded work |
| game-developer | Game development |
| shopify-expert | Shopify project |

## Process Skill Reference (Architect tier only)

These skills are ONLY recommended for Architect-tier tasks. Never recommend them for Quick or Build tasks.

| Skill | What it does | When to recommend at Architect tier |
|-------|-------------|-------------------------------------|
| brainstorming | Collaborative design exploration before coding | Always for Architect — this is how you figure out what to build |
| writing-plans | Creates step-by-step implementation plan | Always for Architect — multi-file work needs a plan |
| systematic-debugging | Structured debugging with root cause analysis | When the task is investigating a complex, hard-to-reproduce bug |
| test-driven-development | Red-green-refactor TDD cycle | When the task explicitly needs test coverage or is high-risk logic |
| verification-before-completion | Sanity check before claiming done | Always for Architect — too many moving parts to skip this |
| requesting-code-review | Structured code review request | When the work will be PR'd and needs thorough review |

## Heads Up Logic

After listing skill recommendations, check for these blind spots. Only include warnings that are actually relevant to the task.

| Condition | Warning |
|-----------|---------|
| No test directory and no test files found | "No tests detected — consider adding coverage for critical paths." |
| Task involves user data, auth, or personal info | "This touches sensitive data — a security review is worth the time." |
| No CI/CD config | "No CI pipeline detected — consider automating checks." |
| `.env` files committed or no `.gitignore` | "Check that secrets aren't being committed." |
| No `CLAUDE.md` in the project root | "Consider running `/setup` to generate a CLAUDE.md for this project." |

## Key Principles

- **Size down, not up** — most tasks need less process than you think
- **Framework skills are free** — recommend them whenever the stack matches, regardless of tier
- **Process skills are expensive** — only recommend for Architect tier
- **Contextual over generic** — explain why each skill matters for THIS task
- **The user decides** — if they override your tier, respect it immediately
- **`/pair` is the sweet spot** — for anything between "just do it" and "full pipeline," pair programming with approval gates gives control without ceremony
