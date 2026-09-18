# solo-builder-workflow

Using Beavy as the contract-of-record when you're building both sides of an API yourself. Even with no second team to coordinate with, your own frontend code and your own backend code can still drift apart — usually because an AI assistant changed something three prompts after you last checked.

## What this shows

- Using a Beavy contract as your own single source of truth across sessions, instead of relying on memory of "what the API was supposed to return."
- Catching the specific solo failure mode: you (or your AI assistant) change the backend's response shape while iterating, and your already-written frontend code silently breaks against it.

## Prerequisites

- A free Beavy account.
- Claude Code or another MCP-compatible assistant, connected via `.mcp.json` (see [`hello-contract/`](../hello-contract)).

## The problem this solves

When you're building solo with an AI assistant, a normal session looks like: describe the backend, get code, iterate on it, describe the frontend, get code, iterate on that too. Nothing forces the shape you described in step 1 to still match what got built by step 6 — an assistant refactoring the backend later in the session (renaming a field, changing a status code, restructuring nested data) has no way to know your frontend code five files away is still assuming the original shape. You find out when something breaks, not before.

## Walkthrough

**1. Lock in the contract before writing either side**

> "Using Beavy, create a contract for a Habit Tracker API. Add a GET /habits endpoint returning an array of objects, each with `id` (number), `name` (string), and `streak_days` (number)."

This is now ground truth independent of your session's memory — it persists even across a new chat, a new day, or a model switch.

**2. Build the frontend against the mock, immediately**

You don't have to build the backend first. Point your frontend code at the live mock:

```bash
curl https://beavy.beavermaster.com/mock/<endpoint-id>/habits
```

```json
[
  { "id": 1, "name": "Read", "streak_days": 12 },
  { "id": 2, "name": "Run", "streak_days": 4 }
]
```

Write and finish your frontend component against this today — your own future backend doesn't block you.

**3. Build the real backend later — possibly in a different session**

Days later, in a fresh conversation, ask your assistant to implement the real `/habits` endpoint. Because the contract lives in Beavy (not in that conversation's context), you can hand it the contract instead of re-describing the shape from memory:

> "Implement GET /habits per the contract already defined in Beavy for the Habit Tracker API."

**4. Validate before you trust it**

> "Validate my real /habits endpoint against the Beavy contract."

If the assistant implementing the backend took a shortcut — returned `streak` instead of `streak_days`, or nested the array under a `data` key — validation catches it now, against the same contract your frontend was actually built against, instead of you noticing the UI looks wrong later and having to guess why.

## Why this matters even solo

The "even when it's just you" framing isn't about team size — it's about session boundaries. Every new AI conversation is effectively a new "team member" with no memory of what the last one agreed to. A Beavy contract is the thing that persists across that boundary. See [`fe-be-drift-demo/`](../fe-be-drift-demo) for the same underlying mechanism applied to two actual people instead of two sessions.

## What's next

- [`hello-contract/`](../hello-contract) — if you haven't done the minimal walkthrough yet, start there.
- [`fe-be-drift-demo/`](../fe-be-drift-demo) — the team version of this same drift problem.
