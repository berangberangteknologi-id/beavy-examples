# beavy-examples

**ONE CONTRACT. NO DRIFT.**

Runnable, end-to-end projects showing how to use [Beavy](https://beavy.beavermaster.com/)'s MCP server to define an API contract with your AI coding assistant, get an instant mock for whoever's waiting on the other side, and validate the real backend against that same contract before it ships. Every example here is a real project you can clone and run — not slides.

## What is Beavy

Beavy is a contract-coordination tool for developers building with AI coding assistants. You define an API contract with Claude Code, Cursor, or any MCP-compatible assistant; Beavy instantly serves a mock for whoever's waiting on the other side of that API, and continuously validates the real endpoint against the same contract once it ships — catching silent drift when an AI assistant changes a response shape. No existing OpenAPI spec required. Free plan: 50 endpoints / 300 validations per month / 3 seats. Works solo or across split frontend/backend teams.

## 30-second quickstart

1. Sign up free — no card required: [beavy.beavermaster.com](https://beavy.beavermaster.com/)
2. Point your MCP-compatible assistant at Beavy by adding this to your project's `.mcp.json`:

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

3. Open your assistant (Claude Code, Cursor, etc.) in that project — the first Beavy tool call triggers a browser OAuth prompt to link your account.
4. Ask your assistant to define a contract, e.g. *"Using Beavy, create a contract for a GET /users/:id endpoint that returns id, name, and email."*
5. Curl the mock Beavy just stood up for that endpoint. You're now unblocked — build against it before the real backend exists.

That's the whole loop: **define → mock → validate**. The examples below walk through each variant of it.

## Examples

| Example | What it shows |
|---|---|
| [`hello-contract/`](./hello-contract) | The minimal single-endpoint walkthrough: define a contract via your AI assistant, get an instant mock, curl it. Start here. |
| [`fe-be-drift-demo/`](./fe-be-drift-demo) | The flagship story: a frontend built against a mock, a backend that later changes a field shape, and Beavy's validate step catching the drift before it ships. |
| [`cursor-quickstart/`](./cursor-quickstart) | The same define → mock → validate flow, run through Cursor instead of Claude Code — Beavy is MCP-standard, not tied to one assistant. |
| [`solo-builder-workflow/`](./solo-builder-workflow) | Using Beavy as the contract-of-record when you're building both sides yourself — keeping your own future backend honest against what your frontend already expects. |

Frontend/backend split teams are Beavy's primary use case — `fe-be-drift-demo` is the one to read first if that's your situation. `solo-builder-workflow` covers the "even when it's just you" case.

## How it works

Three steps from conversation to validated API.

1. **Define the contract with AI** — Use an MCP-compatible AI assistant — like Claude Code — to describe your API. Beavy creates a structured contract with endpoints, schemas, and expected responses — no YAML, no boilerplate.
2. **Beavy serves a mock instantly** — The moment the contract is saved, Beavy spins up a live mock endpoint. Frontend and integration tests can call it immediately, unblocking parallel development.
3. **Point Beavy at your real API and it validates** — When your real API is running, ask Beavy to validate. It calls your endpoint, checks the response against the contract schema, and reports any drift — automatically.

## FAQ

**What's the difference between Free and Pro?**
Free covers 50 endpoints, 300 validations/mo, 7,500 mock requests/mo, and 3 seats — enough to fully evaluate Beavy on a real project, no card required. Pro raises those to 300 endpoints, 7,500 validations/mo, 30,000 mock requests/mo, still with 3 seats included, plus the option to add more.

**Do I need an existing OpenAPI spec to use Beavy?**
No. Describe your API in plain conversation to any MCP-compatible AI assistant (Claude Code, Cursor, etc.) and Beavy builds the structured contract for you — no YAML, no manual spec-writing.

**Which AI assistants does Beavy work with?**
Any assistant that supports MCP (Model Context Protocol), including Claude Code and Cursor. Every Beavy action — define a contract, serve a mock, run a validation — is exposed as an MCP tool call, so you never have to open the Beavy UI if you don't want to.

**How does drift validation actually work?**
Point Beavy at your live endpoint and it calls it directly, comparing the real response — status code, schema, field types — against your saved contract. Any mismatch is flagged immediately, so you catch a silent break before a teammate or customer does.

**What happens when I hit a plan limit?**
Limits are hard blocks, not silent failures — you get a clear error the moment you hit one, never silent data loss. Monthly counters (validations, mock requests) reset on the 1st of each month at midnight UTC; resource caps (endpoints) stay in place until you upgrade.

**What's the difference between monthly and annual billing?**
Monthly bills the full rate each month. Annual bills upfront for 12 months at 17% off (Pro: $5.99/mo → $4.99/mo).

**What payment methods do you accept, and is my payment data safe?**
Card payments worldwide via Paddle, our merchant of record (they handle tax/VAT for you). We never store your card details ourselves.

**How do additional seats work?**
Additional seats are Pro-only, $2.49/seat/mo. Each seat adds +1 seat, +100 endpoints, +2,500 validations/mo, +10,000 mock requests/mo, and activates immediately (prorated cost on your next invoice). Removing a seat takes effect at the end of your current billing period — if you're then over your endpoint cap, your newest endpoints are frozen (not deleted, data kept) until you add capacity back, and you get a one-time choice of which stay active.

**Can I cancel or change plans anytime?**
Yes — upgrade, downgrade, or cancel anytime from your billing settings, no long-term contract. Downgrades and cancellations take effect at the end of your current billing period.

## Links

- Product: [beavy.beavermaster.com](https://beavy.beavermaster.com/)
- Full site summary for LLMs/crawlers: [beavy.beavermaster.com/llms-full.txt](https://beavy.beavermaster.com/llms-full.txt)
- Support: support@beavy.beavermaster.com

## License

Code and content in this repository are licensed under the [MIT License](./LICENSE). This covers the examples and docs here only — it does not apply to the Beavy product itself.

## Contributing

Found a bug in an example, or want to add one for a stack/assistant we don't cover yet? Open an issue or a PR — see [CONTRIBUTING.md](./CONTRIBUTING.md).
