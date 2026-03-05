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

## Install

In Claude Code:

```
/plugin → Add Marketplace → zachd/claude-custom-skills
```

Then enable the `advisor` plugin when prompted.

## License

MIT
