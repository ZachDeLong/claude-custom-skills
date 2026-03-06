---
name: retro
description: "Use after finishing work in a session. Reviews the conversation history to identify what went well, what didn't, skills used, and learnings worth saving."
metadata:
  author: zachd
  version: "1.0.0"
---

# Session Retrospective

## Overview

Review the current session's conversation history and produce an honest, specific retrospective. Analyze what the user set out to do, what actually happened, what worked, what didn't, and what's worth remembering for future sessions.

<HARD-GATE>
Do NOT give a generic or vague retrospective. Every point must reference something SPECIFIC that happened in this session.
Do NOT skip the analysis. Read the full conversation history before outputting anything.
Do NOT sugarcoat failures. If something took 4 attempts, say so. If time was wasted, say so.
</HARD-GATE>

## Process

### Step 1: Analyze the session

Silently review the full conversation history. Identify:

- **The goal:** What did the user set out to do?
- **The outcome:** Was it achieved? Partially? Not at all?
- **What worked:** Things that succeeded on the first try, good decisions, effective approaches
- **What didn't work:** Errors, dead ends, tangents, things that needed multiple attempts, wasted effort
- **Skills used:** Which skills were invoked and whether they actually helped
- **Tools used:** Key tool calls — what was effective, what was unnecessary
- **Surprises:** Anything unexpected that came up (gotchas, undocumented behavior, things that should be documented)

### Step 2: Output the retrospective

Use this template:

```
## Session Retrospective

### Goal
[One sentence: what the user set out to do]

### Outcome
[Was it achieved? What's the current state?]

### What Went Well
- [Specific thing that worked — reference what actually happened]
- [Another specific thing]

### What Didn't Go Well
- [Specific problem — what happened, how many attempts, how it was resolved]
- [Another specific problem]

### Patterns & Learnings
- [Reusable insight for future sessions]
- [Gotcha discovered that should be remembered]
- [Convention or approach that worked well]

### Skills Used
| Skill | Helped? | Notes |
|-------|---------|-------|
| skill-name | Yes/No/Partially | [Brief note on how it helped or didn't] |

### What's Left
- [Unfinished work or follow-ups needed]
- [Or "Nothing — all work completed" if done]

---
Want me to save any of these learnings to your CLAUDE.md or memory files?
```

### Step 3: Be specific, not generic

Every bullet point must reference something that actually happened in this session.

Good: "Setting up the local plugin marketplace took 4 attempts — we tried manual JSON editing, then discovered /plugin → Add Marketplace supports local directory paths"

Bad: "Plugin setup had some challenges"

Good: "The brainstorming skill kept the design focused and prevented us from jumping into implementation too early"

Bad: "Planning was helpful"

Good: "The `disable-model-invocation: true` flag in command frontmatter prevents the Skill tool from loading the skill — discovered this after the first test failed"

Bad: "We learned some things about skill configuration"

### Step 4: Offer to save learnings

After the retrospective, ask if the user wants to save any learnings. If they say yes:

- **CLAUDE.md updates:** Add project-specific conventions, gotchas, or patterns to the project's CLAUDE.md
- **Memory file updates:** Add cross-project learnings to the user's memory files (check for existing memory directory)
- **Neither:** Just leave the retro in chat

Only save what the user explicitly approves. Don't auto-save anything.

## Key Principles

- **Honesty over comfort** — A retro that only says "great job" is useless. Be candid about what went wrong.
- **Specific over generic** — Every point must reference this session, not general truisms.
- **Actionable over descriptive** — "Add X to CLAUDE.md" beats "we should document things better."
- **Concise** — Keep each bullet to 1-2 sentences. The retro should be scannable.
- **Complete** — Don't skip sections. If nothing went wrong, say "Nothing significant — session went smoothly." Don't just omit the section.
