# Synapse Compiler

Synapse is a compiler for AI agent workflows. It turns a workflow into a typed graph, measures it, rewrites it to run
faster or cheaper, and refuses any rewrite that fails an equivalence check. Steps that change the outside world, such
as payments and emails, are never reordered, repeated or cached.

## What this plugin adds

- A skill that tells Claude how to compile, optimize and check a workflow.
- Slash commands: `optimize`, `why`, `waste` and `evidence`.
- The `synapse` MCP server, with tools for each step.

## What it runs, sends and stores

The MCP server runs on your machine with `uvx --from halfabyte-synapse==0.3.6 synapse mcp`, which downloads that
exact package from PyPI, so you need [uv](https://docs.astral.sh/uv/). It stores graphs and measurements in a `data`
folder in your working directory.

It sends nothing by default. If you set `SYNAPSE_API_KEY` to a Synapse Cloud project key, the `cloud_dashboard` and
`cloud_run` tools read that project over HTTPS. Tools that run a workflow call the model provider that workflow is
configured for, on your own key. It never runs a workflow live: external steps are stubbed, irreversible ones refused.

## Licence

MIT.
