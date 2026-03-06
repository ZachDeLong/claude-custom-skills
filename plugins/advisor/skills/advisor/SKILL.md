---
name: advisor
description: "Use when starting a project, planning a feature, or unsure which skills to use. Scans your repo and recommends the right skills for your situation with plain-language explanations."
metadata:
  author: zachd
  version: "1.1.0"
---

# Skill Advisor

## Overview

Help developers figure out which Claude Code skills to use. Scan the repo for context, match against the user's goals, and recommend skills organized by phase — before coding, during implementation, and before shipping. Explain each recommendation in plain language tied to the user's specific situation, not generic skill descriptions.

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

### Step 3: Match and recommend

Cross-reference the user's description with the detected stack. Consult the Skill Taxonomy below to pick the right skills for their situation.

### Step 4: Output recommendations

Use this exact template format:

```
## Skill Recommendations for: [what they described]

**Stack detected:** [frameworks, languages, services found in repo]

### Before You Code
- **skill-name** — [1-2 sentence contextual explanation of why this skill helps for THEIR specific situation]

### During Implementation
- **skill-name** — [contextual why]

### Before Shipping
- **skill-name** — [contextual why]

### Workflow & Shipping
- **skill-name** — [contextual why]

### Suggested Workflow
1. **skill-name** → [short action phrase]
2. **skill-name** → [short action phrase]
3. **skill-name** → [short action phrase]

> **To start:** "[natural one-liner the user can paste to kick off the workflow]"

### Heads Up
- [Non-skill callout about blind spots — see Heads Up Logic below]

---
Want me to go deeper? I can ask a few questions to refine these recommendations.
```

### Step 5: Make recommendations contextual

Each skill recommendation MUST include a contextual explanation — not a generic description of the skill, but why it matters for what THIS user is building.

Good: "**nextjs-developer** — Your app uses the Next.js App Router, and adding this feature will likely involve server components and route handlers"

Bad: "**nextjs-developer** — Helps with Next.js development"

Good: "**secure-code-guardian** — Since this feature handles user profile data with Supabase RLS, it's worth checking your auth and data access patterns"

Bad: "**secure-code-guardian** — Checks security patterns"

Only include sections that have relevant skills. If nothing fits "Before You Code" for a straightforward bug fix, skip that section. Every section shown must have at least one recommendation.

Filter aggressively. If there's no Python in the project, do not recommend python-pro. If the task doesn't involve AI, do not recommend prompt-engineer. Fewer, more relevant recommendations beat a long list.

### Step 6: Add a suggested workflow

After the skill categories, add a `### Suggested Workflow` section that orders 4-6 key skills into an actionable sequence. This tells the user "run these in this order."

**Rules:**
- Pick only the skills that form a natural sequence — not every recommended skill needs to appear
- Use short action phrases after the arrow: `→ scope the feature` not the full contextual explanation
- If the task is too simple for a workflow (e.g. one or two skills), skip this section entirely

**Common sequences to recognize and adapt:**
- **New feature:** brainstorming → writing-plans → [framework skill] → verification-before-completion → commit-push-pr
- **Bug fix:** systematic-debugging → [framework skill] → test-master → verification-before-completion → commit
- **Refactor:** architecture-designer → writing-plans → [framework skill] → simplify → requesting-code-review → commit-push-pr
- **Security audit:** secure-code-guardian → security-reviewer → verification-before-completion → commit

Adapt these to the user's specific situation. The workflow should feel like a step-by-step guide, not a generic template.

**After the workflow, add a copy-paste kickoff line:**

End the Suggested Workflow section with a one-liner the user can paste to start the workflow. Build it from the workflow steps.

Example: `> **To start:** "Use brainstorming to scope this feature, then writing-plans to create the implementation plan, implement with typescript-pro and nextjs-developer, run verification-before-completion before shipping, and commit-push-pr when done."`

Keep it natural — one sentence, not a list. The user should be able to paste it directly into a new session or the current one.

## Phase 2: Refinement

If the user says yes to going deeper:

### Ask 2-3 clarifying questions

Ask targeted, context-aware questions in plain language. Do not ask about things that are already obvious from the repo scan or user's description.

Use these as a starting point and adapt based on context:

- "Are you building something new or changing something that already exists?"
- "What parts does this affect — how things look, how data is stored, backend logic, or a mix?"
- "Does this involve user accounts, personal info, or login?"
- "Does this feature use AI to generate or analyze anything?"
- "Do you want to make sure this is tested before shipping?"
- "Is this going to production soon, or is it more experimental?"
- "Are there performance concerns — large datasets, real-time updates, many concurrent users?"

Pick only the questions whose answers would actually change your recommendations. If the repo scan already tells you it's a Next.js project with Supabase auth, don't ask about the stack or whether it involves login.

### Output refined recommendations

After the user answers, output updated recommendations in the same template format. Skills may be added, removed, or reordered based on the new information. Briefly note what changed and why if it's not obvious.

## Skill Taxonomy

This is the knowledge base for matching. Each entry has: name, what it does, and when to recommend it.

### Before Coding

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| brainstorming | Helps think through what you're building before writing code. Collaborative dialogue to turn ideas into designs. | Starting any non-trivial feature. |
| writing-plans | Creates a step-by-step implementation plan with exact file paths and code. | Feature touches multiple files or has several moving parts. |
| feature-dev | Guided feature development with architecture focus. Understands the codebase first. | Larger features that need deep codebase understanding. |
| architecture-designer | Helps design system architecture — components, data flow, boundaries. | Big structural decisions, new services, or major refactors. |
| the-fool | Challenges your ideas and plays devil's advocate. Pokes holes in your approach. | When you want someone to stress-test your plan before you build it. |
| common-ground | Surfaces hidden assumptions between you and Claude. | When you want to make sure you're both on the same page before starting. |

### During Implementation — Frontend

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| nextjs-developer | Next.js App Router, server components, middleware, route handlers. | `package.json` has `"next"` |
| react-expert | Component architecture, hooks, state management, React patterns. | `package.json` has `"react"` |
| vue-expert | Vue 3 Composition API, Nuxt 3, Vue ecosystem. | `package.json` has `"vue"` |
| angular-architect | Angular standalone components, signals, RxJS. | `package.json` has `"@angular/core"` |
| frontend-design | Creates polished, production-grade UI. Layout, styling, responsive design. | Task involves visual design or UI work. |
| react-native-expert | Cross-platform mobile apps with React Native. | `package.json` has `"react-native"` |
| flutter-expert | Cross-platform apps with Flutter/Dart. | `pubspec.yaml` exists. |

### During Implementation — Backend

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| python-pro | Python type safety, async, modern patterns, project structure. | `requirements.txt`, `pyproject.toml`, or `.py` files present. |
| fastapi-expert | FastAPI async APIs, dependency injection, Pydantic models. | Dependencies include `"fastapi"`. |
| django-expert | Django web apps, REST APIs, ORM, admin. | Dependencies include `"django"`. |
| golang-pro | Go concurrency, microservices, idiomatic Go patterns. | `go.mod` exists. |
| rust-engineer | Rust memory safety, ownership, systems programming. | `Cargo.toml` exists. |
| java-architect | Spring Boot, enterprise Java patterns, JVM ecosystem. | `pom.xml` or `build.gradle` exists. |
| csharp-developer | C#/.NET applications, ASP.NET, Entity Framework. | `.csproj` files exist. |
| kotlin-specialist | Kotlin coroutines, multiplatform, modern JVM patterns. | `build.gradle.kts` with kotlin. |
| php-pro | Modern PHP 8.3+, best practices. | `composer.json` exists. |
| rails-expert | Rails 7+ with Hotwire, Turbo, Stimulus. | `Gemfile` has `"rails"`. |
| laravel-specialist | Laravel 10+, Eloquent, Blade, queues. | `composer.json` has `"laravel"`. |
| nestjs-expert | NestJS modular architecture, decorators, pipes. | `package.json` has `"@nestjs/core"`. |
| dotnet-core-expert | .NET 8 minimal APIs, modern .NET patterns. | `.csproj` with `net8`. |
| spring-boot-engineer | Spring Boot 3.x, auto-configuration, actuator. | `pom.xml` with `spring-boot`. |
| swift-expert | iOS/macOS with SwiftUI, Combine, modern Swift. | `.xcodeproj` or `Package.swift` exists. |
| wordpress-pro | WordPress themes, plugins, hooks, custom post types. | `wp-content` directory or `style.css` with Theme Name. |

### During Implementation — TypeScript / JavaScript

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| typescript-pro | Advanced TypeScript type systems, generics, utility types. | `tsconfig.json` exists. |
| javascript-pro | Modern JavaScript ES2023+, async patterns. | `.js` files without TypeScript. |

### During Implementation — Data & AI

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| prompt-engineer | Designing prompts for LLMs, optimizing AI output quality. | Task involves AI/LLM features. |
| rag-architect | Building RAG systems with vector databases and embeddings. | Task involves knowledge retrieval combined with AI. |
| ml-pipeline | ML training pipelines, model lifecycle, experiment tracking. | Task involves model training. |
| fine-tuning-expert | Fine-tuning LLMs for specific tasks. | Task involves custom model training. |
| claude-developer-platform | Building with the Claude API or Anthropic SDK. | Code imports `anthropic` SDK or task involves Claude integration. |
| pandas-pro | DataFrame operations, data cleaning, analysis pipelines. | Using pandas for data processing or analysis. |

### During Implementation — Database

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| postgres-pro | PostgreSQL optimization, replication, RLS, advanced features. | Using PostgreSQL or Supabase. |
| sql-pro | SQL query optimization, schema design, migrations. | Any SQL database. |
| database-optimizer | Query analysis, indexing strategy, performance tuning. | Performance issues with database. |

### During Implementation — API Design

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| api-designer | REST/GraphQL API design, OpenAPI specs, endpoint conventions. | Building new APIs. |
| graphql-architect | GraphQL schemas, resolvers, Apollo Federation. | Using GraphQL. |
| websocket-engineer | Real-time communication with WebSockets, connection management. | Building real-time features (chat, live updates, etc.). |
| mcp-developer | Building MCP servers and clients for AI tool integrations. | Building AI tool integrations with Model Context Protocol. |

### During Implementation — Infrastructure

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| devops-engineer | CI/CD pipelines, containers, deployment automation. | `Dockerfile`, `.github/workflows/`, or CI config present. |
| terraform-engineer | Infrastructure as code with Terraform. | `.tf` files present. |
| kubernetes-specialist | Kubernetes workloads, pod configuration, scaling. | K8s manifests or Helm charts present. |
| cloud-architect | Multi-cloud architecture, service selection, cost optimization. | Cloud infrastructure decisions. |
| monitoring-expert | Observability, logging, metrics, alerting. | Setting up monitoring or debugging production issues. |
| sre-engineer | SLIs/SLOs, reliability engineering, incident response. | Production reliability work. |

### During Implementation — CLI & Specialized

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| cli-developer | Building command-line tools, argument parsing, output formatting. | Task is a command-line tool. |
| embedded-systems | Firmware, RTOS, hardware interfaces. | Microcontroller or embedded work. |
| game-developer | Game systems, Unity/Unreal, game loops, physics. | Game development. |

### During Implementation — Domain-specific

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| shopify-expert | Shopify themes, apps, Liquid templating, Shopify APIs. | Shopify project. |
| salesforce-developer | Apex, Lightning Web Components, SOQL. | Salesforce project. |
| spark-engineer | Apache Spark data processing, distributed computing. | Big data pipelines. |

### Before Shipping

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| verification-before-completion | Sanity check before claiming work is done. Catches missed edge cases, broken imports, untested paths. | Always recommend for non-trivial work. |
| secure-code-guardian | Checks auth, data access, and security patterns. | Handling user data, auth, or personal info. |
| security-reviewer | Full security audit for vulnerabilities — injection, XSS, CSRF, etc. | Periodic security reviews or before major releases. |
| test-master | Helps write tests and build test strategies. Unit, integration, and e2e. | Adding test coverage or building a testing strategy. |
| playwright-expert | End-to-end testing with Playwright for browser-based flows. | Writing browser-based tests. |
| code-reviewer | Reviews code for bugs, quality, and convention adherence. | Before merging any significant changes. |
| requesting-code-review | Structures a code review request with context and focus areas. | When you want thorough review of your changes. |
| receiving-code-review | Processes code review feedback systematically. | When you receive PR comments and need to address them. |
| simplify | Reviews changed code for reuse, quality, and efficiency. Quick cleanup. | Quick cleanup pass before shipping. |

### Workflow & Shipping

| Skill | What it does | When to recommend |
|-------|-------------|-------------------|
| commit | Creates structured, conventional git commits. | Any commit. |
| commit-push-pr | Commits, pushes, and opens a PR in one flow. | When ready to ship a feature or fix. |
| finishing-a-development-branch | Guides merge/PR decisions at the end of a branch. | When a feature branch is done. |
| executing-plans | Follows a written implementation plan step by step. | Use in a separate session to execute plans written by writing-plans. |
| subagent-driven-development | Dispatches fresh agents per task from a plan. | Multi-task plans executed in one session. |
| dispatching-parallel-agents | Runs independent tasks simultaneously. | When tasks don't depend on each other. |
| claude-md-improver | Audits and improves CLAUDE.md files. | Improving project documentation. |
| revise-claude-md | Updates CLAUDE.md with learnings from the current session. | After significant sessions where new patterns were discovered. |

## Heads Up Logic

After listing skill recommendations, check for these common blind spots and include relevant warnings in the "Heads Up" section:

| Condition | Warning |
|-----------|---------|
| No test directory (`tests/`, `__tests__/`, `spec/`) and no test files found | "No tests detected — consider adding coverage for critical paths before this gets more complex." |
| Task involves user data, auth, or personal info | "This touches sensitive data — a security review is worth the time before shipping." |
| No CI/CD config (`.github/workflows/`, `Jenkinsfile`, `.circleci/`, etc.) | "No CI pipeline detected — consider automating checks so bugs don't slip through." |
| Large files (500+ lines) in the area being modified | "Some files in this area are large — might be worth breaking them up for maintainability." |
| `.env` files are committed or no `.gitignore` exists | "Check that secrets aren't being committed — look for `.env` files in version control." |
| No `CLAUDE.md` in the project root | "Consider adding a CLAUDE.md to capture project conventions — it helps Claude give better answers in future sessions." |

Only include warnings that are actually relevant. Don't list all of them every time.

## Key Principles

- **Contextual over generic** — "Helps structure your Supabase RLS policies for the new user roles" beats "Helps with database optimization."
- **Filter aggressively** — Only recommend skills that match the detected stack and described task. Fewer, better recommendations.
- **Plain language** — The audience may not know the skill ecosystem. No jargon, no assumed knowledge.
- **Scannable output** — Developers skim. Use the template format, keep explanations to 1-2 sentences.
- **Chat output only** — Never write files, never invoke other skills, never take action. Recommend and explain.
