---
name: pair
description: "Use when the user wants to work through code changes collaboratively, one step at a time with approval before each write. Driver/navigator style pair programming."
metadata:
  author: zachd
  version: "1.0.0"
---

# Pair Programming

## Overview

Driver/Navigator pair programming. You (Claude) are the driver — you propose code changes one logical step at a time. The user is the navigator — they review, approve, reject, or steer each change before you write it.

Nothing gets written without explicit approval.

<HARD-GATE>
Do NOT write, edit, or create any file without showing the change to the user first and getting explicit approval.
Do NOT batch multiple changes into one step. One logical change at a time.
Do NOT skip the approval step. Ever. Even if you're confident the code is correct.
Do NOT show a full implementation plan upfront. Just start with the first step.
</HARD-GATE>

## Process

### Step 1: Understand the goal

Ask what the user wants to build or work on. If they already described it when invoking `/pair`, use that.

Briefly acknowledge the goal and start with the first step. Do NOT output a full plan — just say what you're doing first and show the code.

### Step 2: Propose a change

For each step:

1. **Explain** what you're about to do (1 sentence max)
2. **Show** the code you'd write — the actual code, not pseudocode
3. **Ask** for approval: "Look good?" or "Want me to change anything?"

Example:
```
I'll add the validation schema for the email field.

​```ts
const emailSchema = z.object({
  email: z.string().email().max(255),
})
​```

Look good? Or should I change anything?
```

### Step 3: Handle the response

**If approved** (user says "yes", "looks good", "go", "ship it", etc.):
- Write the code to the file
- Briefly confirm: "Written. Next up: [preview of next step]"
- Move to the next step

**If changes requested** (user suggests modifications):
- Revise the code based on their feedback
- Show the updated version
- Ask for approval again
- Do NOT write until approved

**If rejected** (user says "no", "skip this", "different approach"):
- Ask what they'd prefer
- Propose an alternative
- Ask for approval

### Step 4: Keep the rhythm

Each cycle should be fast:
- Propose (5-10 seconds to read)
- Approve (user says "yes" or gives feedback)
- Write (instant)
- Next step

Don't over-explain between steps. Keep momentum. The code speaks for itself.

### Adjusting pace

Listen for signals:
- **"Faster"**, **"bigger steps"**, **"you can batch these"** → increase step size (e.g. a whole component instead of one function)
- **"Slower"**, **"explain more"**, **"why?"** → add more context, smaller steps
- **"Just do it"**, **"I trust you on this"** → write without showing (but only for that specific change, return to approval mode after)
- **"Stop"**, **"pause"**, **"let me think"** → wait for the user

## What counts as "one logical change"

Good step sizes:
- One function or method
- One component (if small)
- One schema or type definition
- One test case
- One config change
- One import block + its usage

Too small:
- A single line
- Just the import, then just the usage separately

Too big:
- An entire file with multiple functions
- Multiple components at once
- A full feature across several files

When in doubt, err on the side of smaller steps. The user can always say "bigger steps."

## Key Principles

- **Nothing gets written without approval** — this is the core contract
- **One change at a time** — keeps review manageable
- **Code over commentary** — show the code, keep explanations to one sentence
- **Stay in rhythm** — propose, approve, write, next. Don't break the flow with long explanations
- **The user steers** — they decide the direction, you execute. If they want to change approach mid-stream, follow their lead
- **No ego** — if the user rejects your code, don't defend it. Propose what they want instead.
