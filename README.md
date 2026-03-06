# Claude Custom Skills

A collection of custom skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Skills

### `/advisor`

Recommends which Claude Code skills to use for your project or feature. Scans your repo, detects your stack, and gives categorized recommendations with plain-language explanations.

**How it works:**
1. You describe what you're building or working on
2. It scans your repo (package.json, CLAUDE.md, directory structure, config files)
3. Outputs skill recommendations organized by phase: Before You Code → During Implementation → Before Shipping → Workflow & Shipping
4. Flags blind spots (no tests, security concerns, missing CI, etc.)
5. Optionally asks clarifying questions to refine recommendations

**Example output:**
```
## Skill Recommendations for: Improving the essay critique feature

Stack detected: Next.js 16, TypeScript, Supabase, Vercel, Claude Haiku

### Before You Code
- **brainstorming** — Helps scope what "improve" means before you start building

### During Implementation
- **prompt-engineer** — Your critique quality depends on the Claude Haiku prompt
- **nextjs-developer** — The /api/essay-critique route needs server-side work
- **typescript-pro** — Zod schemas define what the AI can return

### Before Shipping
- **secure-code-guardian** — Shareable critique links are publicly readable

### Heads Up
- No tests detected — consider adding coverage for the critique flow
```

### `/retro`

Session retrospective — run after finishing work to review what happened. Analyzes the full conversation history and outputs an honest, specific retrospective.

**What it covers:**
- What was the goal and was it achieved
- What went well (first-try successes, good decisions)
- What didn't go well (errors, dead ends, wasted effort)
- Patterns and learnings worth remembering
- Skills used and whether they helped
- What's left to do

Optionally saves key learnings to your CLAUDE.md or memory files.

### `/setup`

Project setup — scans your repo and generates a minimal, practical CLAUDE.md. Like what a senior dev would write in 5 minutes to onboard a teammate.

**What it generates:**
- Stack one-liner (framework + language + database + deployment)
- Commands (dev, build, test, lint — pulled from package.json/Makefile/etc.)
- Key paths (important directories and files)
- Conventions (patterns detected from actual code)
- Environment variables (from .env.example)
- Plugin recommendations for your stack

Works on new projects (generates from scratch) and existing projects (suggests improvements to existing CLAUDE.md).

### `/pair`

Pair programming — Claude drives, you navigate. Claude proposes one logical change at a time (a function, a component, a schema), shows you the code, and waits for your approval before writing anything.

**The rhythm:**
1. Claude explains what it's about to do (1 sentence)
2. Shows the code
3. You approve, request changes, or reject
4. If approved, Claude writes it and moves to the next step

Say "faster" for bigger steps, "slower" for more explanation, or "just do it" to let Claude run on a specific change.

### `/checkpoint`

Save a snapshot of where you are mid-session. Records what's done, what's in progress, what's next, and what to watch out for. Saves to `.claude/checkpoint.md` so your next session can pick up automatically.

**What it saves:**
- Completed work
- In-progress items (with current state)
- What's next (in priority order)
- Gotchas and important context
- Uncommitted changes and files touched

The next session auto-detects the checkpoint and offers to resume where you left off.

## Install

In Claude Code:

```
/plugin → Add Marketplace → ZachDeLong/claude-dev-workflow
```

Then enable the plugins you want (`advisor`, `retro`, `setup`, `pair`, `checkpoint`) when prompted.

## License

MIT
