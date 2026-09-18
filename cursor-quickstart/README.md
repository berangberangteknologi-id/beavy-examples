# cursor-quickstart

The same define → mock → validate flow as [`hello-contract/`](../hello-contract), run through [Cursor](https://cursor.com/) instead of Claude Code. Beavy is MCP-standard — it works with any assistant that speaks MCP, not just one.

## What this shows

- Connecting Beavy to Cursor via the same `.mcp.json` shape used for Claude Code — no assistant-specific setup on Beavy's side.
- That the underlying contract → mock → validate loop is identical regardless of which AI assistant is driving it.

## Prerequisites

- A free Beavy account.
- [Cursor](https://cursor.com/), with MCP support enabled (Cursor Settings → MCP).

## Setup

Cursor reads MCP server configuration from the same `.mcp.json` file convention. In your project root:

```json
{
  "mcpServers": {
    "beavy": {
      "type": "http",
      "url": "https://beavy.beavermaster.com/mcp"
    }
  }
}
```

Open this folder in Cursor. When Cursor's agent makes its first Beavy tool call, you'll get the same browser OAuth prompt as with Claude Code — approve it once and Cursor has access to your Beavy account for the rest of the session.

## Walkthrough

**1. Define a contract**

In Cursor's agent chat:

> "Using Beavy, create a contract for a Notes API with a POST /notes endpoint. It takes a `title` (string) and `body` (string) and returns the created note with an `id` (number), `title`, `body`, and `created_at` (ISO timestamp string)."

Cursor's agent calls the same underlying Beavy MCP tools (`create_project`, `create_contract`, `create_endpoint`, and the request/response schema tools) as any other MCP client — the tool surface doesn't change based on which assistant is calling it.

**2. Call the mock**

```bash
curl -X POST https://beavy.beavermaster.com/mock/<endpoint-id>/notes \
  -H "Content-Type: application/json" \
  -d '{"title": "Ideas", "body": "Ship the examples repo"}'
```

Beavy returns a mock response matching the contract's shape — a realistic fake `id` and `created_at`, and your actual submitted `title`/`body` echoed back.

**3. Validate a real endpoint (once you've built one)**

> "Point Beavy at my real POST /notes endpoint and validate it against the contract."

Same validation mechanism as every other example in this repo: Beavy calls your live endpoint, diffs the real response against the saved contract, and reports any mismatch.

## Takeaway

Nothing in this walkthrough is Cursor-specific beyond where you type the prompt. The contract lives in Beavy, not in your assistant — switch assistants mid-project (or use different ones on frontend vs. backend) and the contract, the mock, and the validation history all carry over untouched.

## What's next

- [`fe-be-drift-demo/`](../fe-be-drift-demo) — the full frontend/backend drift-catching story.
- [`solo-builder-workflow/`](../solo-builder-workflow) — using Beavy as a single builder across both sides of an API.
