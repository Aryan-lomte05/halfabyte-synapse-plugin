# Synapse Compiler

Synapse is a compiler for AI agent workflows. It turns a workflow into a typed graph, measures it, rewrites it to run
faster or cheaper, and refuses any rewrite that fails an equivalence check. Steps that change the outside world, such
as payments and emails, are never reordered, repeated or cached.

## What this plugin adds

- A skill that tells Claude how to compile, optimize and check a workflow.
- Slash commands: `optimize`, `why`, `waste` and `evidence`.
- The `synapse` MCP server, with tools for each step.

## What it runs, sends and stores

The MCP server runs on your machine with `uv run --frozen`, which installs the exact versions and hashes in
`uv.lock` from PyPI, so you need [uv](https://docs.astral.sh/uv/). It stores graphs and measurements in a `data`
folder in your working directory.

It sends nothing by default. If you set `SYNAPSE_API_KEY` to a Synapse Cloud project key, the `cloud_dashboard` and
`cloud_run` tools read that project over HTTPS. Tools that run a workflow call the model provider that workflow is
configured for, on your own key. It never runs a workflow live: external steps are stubbed, irreversible ones refused.

## Try it

These need no account and no API key. The demo workflows use a built-in stand-in for the model and say so in their output, so their timings show scheduling only.

1. "Run the Synapse demo on research_fanout, then show me what changed and what the firewall decided."
2. "Run the Synapse demo on shop_ops. Which steps did it refuse to touch, and why?"
3. "List Synapse's optimization passes, with the precondition each one must meet."

For your own workflow, compile it or import a trace, then use `/halfabyte-synapse:optimize <graph id>`.

## Troubleshooting

- **The server won't start:** install [uv](https://docs.astral.sh/uv/), then restart your session.
- **No graphs are listed:** graphs are kept per working directory. Run the demo, or compile a workflow here.
- **A workflow can't reach its model:** ask Claude to use the `check_setup` tool, which says which key or model is missing.
- **`cloud_dashboard` fails:** it needs `SYNAPSE_API_KEY` set to a project key.

## Support

Report problems at <https://github.com/Aryan-lomte05/halfabyte-synapse-plugin/issues>. For security reports, see [SECURITY.md](SECURITY.md).

## Privacy

[Privacy policy](PRIVACY.md)

## Licence

MIT.
