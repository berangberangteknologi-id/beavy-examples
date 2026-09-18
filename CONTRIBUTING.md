# Contributing to beavy-examples

Thanks for considering a contribution. This repo exists to show real, runnable ways to use [Beavy](https://beavy.beavermaster.com/) via MCP — the more realistic and stack-diverse the examples, the more useful it is.

## What's welcome

- A new example project showing Beavy used with an AI assistant or stack we don't cover yet (a different language/framework on the "real backend" side, a different MCP-compatible assistant, a different flavor of the drift problem).
- Fixes to an existing example that no longer runs as documented (Beavy's MCP tool surface evolves — see each example's own note on which tools it uses).
- Clarity improvements to any README here.

## What's out of scope

- Changes to the Beavy product itself — this repo only contains example usage, not the Beavy codebase.
- Anything that requires paid-plan features. Every example should run entirely on Beavy's Free plan.

## Adding an example

1. Create a new top-level folder, e.g. `your-example-name/`.
2. Include a `README.md` that follows the shape of the existing examples: what it demonstrates, prerequisites, step-by-step walkthrough, and the exact MCP tool calls used.
3. Make sure it's actually runnable by a stranger who has only just signed up for a free Beavy account — no internal/private assumptions.
4. Open a PR. We'll review for accuracy against the current Beavy MCP tool contract before merging.

## Questions

Open an issue, or reach out at support@beavy.beavermaster.com.
