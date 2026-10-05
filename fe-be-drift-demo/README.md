# fe-be-drift-demo

The flagship Beavy story: a frontend builds against a mock, a backend ships something that quietly doesn't match, and Beavy's validation step catches the drift before it reaches a user. This is the core "one contract, no drift" pitch, end to end.

## What this shows

- A frontend and backend team building in parallel against one shared contract, without blocking on each other.
- A backend change that silently drifts from the agreed contract (a renamed/reshaped field — the most common way "it worked yesterday" breaks).
- Beavy catching that drift the moment you ask it to validate — not when a bug report shows up next sprint.

## Prerequisites

- A free Beavy account.
- Claude Code or another MCP-compatible assistant, connected via `.mcp.json` (see [`hello-contract/`](../hello-contract) for the connection snippet if you haven't set this up yet).
- A toy backend you control, so you can deliberately introduce the drift (any framework — the example below is framework-agnostic).

## Walkthrough

### 1. Define the contract (day 1, before either side is done)

Prompt your assistant:

> "Using Beavy, create a contract for an Orders API. Add a GET /orders/:id endpoint that returns `id` (number), `status` (string), and `total_amount` (number, in cents)."

This is the shared source of truth both teams build against — agreed once, in plain English, instead of a Slack thread and a stale Notion doc.

### 2. Frontend builds against the mock (unblocked immediately)

The moment the contract is saved, Beavy is serving a live mock. The frontend team points their code at it:

```bash
curl https://beavy.beavermaster.com/mock/<endpoint-id>/orders/42
```

```json
{ "id": 42, "status": "shipped", "total_amount": 4599 }
```

Frontend ships UI that reads `total_amount` as cents and formats it as currency. They never had to wait for backend to write a single line of real code.

### 3. Backend ships — and drifts

Backend builds the real `/orders/:id` endpoint independently. Somewhere along the way (an AI assistant refactor, a rename that felt harmless, a "cleaner" field name) the real response comes back as:

```json
{ "id": 42, "status": "shipped", "amount_total": 45.99 }
```

Two silent breaks at once: the field was renamed (`total_amount` → `amount_total`) and the unit changed (cents → dollars). Nothing crashed. Nothing looked wrong in a quick manual test. The frontend, still expecting the original contract, will silently mis-render or fail on this the moment it hits the real endpoint.

### 4. Validate — and catch it

Once the real endpoint is live, prompt your assistant:

> "Point Beavy at my real /orders/:id endpoint and validate it against the contract."

Beavy calls your live endpoint directly and compares the actual response — status code, schema, field names, field types — against the saved contract. The renamed/reshaped field is flagged immediately: the contract expected `total_amount` (number) and the real response has neither that field nor that shape. This is exactly the class of bug that's easy to miss in review and expensive to catch in production.

### 5. Fix and re-validate

Once backend aligns the real response with the contract (or the team deliberately updates the contract to match a new, agreed shape), re-run validation. A clean pass means frontend and backend are provably in sync — not "probably fine," provably.

## Why this is the core Beavy story

This is the same failure mode whether it's two teams or one person wearing both hats — see [`solo-builder-workflow/`](../solo-builder-workflow) for the single-builder version of this exact drift. The mechanism (contract → mock → validate) is identical; only who's on each side changes.

## What's next

- [`cursor-quickstart/`](../cursor-quickstart) — the same define → mock → validate loop through Cursor instead of Claude Code.
