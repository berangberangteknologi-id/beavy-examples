# hello-contract

The minimal Beavy walkthrough: define one endpoint, get an instant mock, curl it. Start here if this is your first time using Beavy.

## What this shows

- Defining a single-endpoint API contract entirely through natural-language prompts to your AI assistant — no manual YAML/JSON contract authoring.
- Beavy serving a live mock the moment the contract is saved.
- Calling that mock like a real API.

## Prerequisites

- A free Beavy account — sign up at [beavy.beavermaster.com](https://beavy.beavermaster.com/), no card required.
- Claude Code (or any MCP-compatible assistant — see [`cursor-quickstart/`](../cursor-quickstart) for the Cursor version of this exact flow).

## Setup

Add Beavy as an MCP server in your project's `.mcp.json`:

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

Open Claude Code in this folder. The first Beavy tool call opens a browser OAuth prompt to link your account — approve it once.

## Walkthrough

**1. Create a project and a contract**

Prompt your assistant:

> "Using Beavy, create a new project called `hello-contract`, then create a contract in it called `Users API`."

Under the hood this calls Beavy's `create_project` and `create_contract` MCP tools. You don't need to know the tool names to do this — describing the intent is enough.

**2. Define the endpoint**

Prompt:

> "Add a GET /users/:id endpoint to that contract. It should return a JSON object with `id` (number), `name` (string), and `email` (string)."

Your assistant calls `create_endpoint` to register the route, then defines the response shape for it — Beavy turns your plain-English description into a structured schema, no manual JSON Schema writing required.

**3. Get the mock**

The moment the endpoint's response schema is saved, Beavy is already serving a mock for it. Ask your assistant for the mock URL, or check the endpoint in the Beavy dashboard, then:

```bash
curl https://beavy.beavermaster.com/mock/<your-endpoint-id>/users/1
```

Expected response — a realistic fake value matching the shape you described:

```json
{
  "id": 1,
  "name": "Ava Thompson",
  "email": "ava.thompson@example.com"
}
```

That's the loop: **describe it in English → Beavy defines the contract → Beavy serves a mock instantly.** Nothing downstream (a frontend, an integration test, a teammate) has to wait for a real backend to exist.

## What's next

- [`fe-be-drift-demo/`](../fe-be-drift-demo) — what happens when the real backend ships something that doesn't match this contract.
- [`solo-builder-workflow/`](../solo-builder-workflow) — using this same loop end-to-end as a single builder.
