---
name: synapse
description: Compile an AI agent workflow into a typed graph, optimize it, and check the result with a firewall. Use when a user wants a workflow faster or cheaper, asks why a step was not optimized, or needs to know if an optimization is safe to ship.
---

# Synapse

Synapse splits a workflow into steps, labels what each step may touch, and only applies
changes that label allows. Use its MCP tools. The commands below are what a person can run in a terminal. Use `python -m synapse` if `synapse` is not
on PATH (on Windows, `py -3 -m synapse`).

## See what an existing agent is doing

Change one import. Calls behave as before, and prompts and answers never leave the process.
```python
from synapse.openai import OpenAI   # instead of: from openai import OpenAI
client = OpenAI(synapse_workflow="support_bot")
```
Run the agent, then `synapse ui` shows the runs on this machine, with no account. With `SYNAPSE_API_KEY`
set to a project key, runs report to Synapse Cloud instead. This reports waste. It rewrites nothing: rewriting
needs a compiled workflow or an imported trace, below.

## MCP tools

If the `synapse` MCP tools are available, prefer them to the shell. They cover the steps below. `run_graph` never
runs live, so `external` steps are stubbed and `irreversible` ones refused. Live runs are for a person, in a terminal.

## Workflow

1. **Get a graph.** Compile a decorated workflow, or import a trace.
   ```
   synapse compile module.path:workflow
   synapse import otel trace.json
   synapse graphs
   ```
2. **Read it.** Note the effects and the effect edges.
   ```
   synapse graph GRAPH_ID
   ```
3. **Optimize it.** This saves a new graph ending in `_opt`.
   ```
   synapse optimize GRAPH_ID
   ```
4. **Explain refusals.**
   ```
   synapse why GRAPH_ID
   synapse why GRAPH_ID --node NODE_ID
   synapse explain PASS_NAME
   ```
5. **Check it.** An optimization with no verdict is a draft.
   ```
   synapse verify GRAPH_ID_opt
   ```
   Built-in demo workflows run here. Real workflows run through
   `synapse bench --workflow NAME --split calib`.

6. **Multi-agent workflows.** See who may overlap, and export the graph for a review.
   ```
   synapse agents GRAPH_ID
   synapse export GRAPH_ID --format mermaid
   ```
7. **Guard a repo.** Pin a baseline, then fail a PR that regresses. Run `synapse doctor` when
   something looks wrong.
   ```
   synapse ci pin WORKFLOW
   synapse ci run WORKFLOW
   ```

## Facts to look up, never to recall

- **Which passes exist, and how many:** run `synapse passes` and read its output. The catalogue changes
  between versions, so never state a count or a pass name from memory.
- **What a pass does and when it is safe:** `synapse explain PASS_NAME`.
- **Whether a model server is safe to overlap:** `synapse audit --model MODEL_ID`. Its result is stored,
  and `optimize` keeps that model's calls one at a time when it is not.

## Rules

- Report the verdict with any speedup. Do not call an optimization safe without APPROVE.
- BLOCK is a useful result. Report the reason and the broken tasks.
- Never pass `--mode live` on a workflow with `external` or `irreversible` steps unless the user
  asks for that run. Those steps change the outside world.
- Say so when numbers come from a demo workflow. Its model is fake, so timings show scheduling
  only.
- A dash means the value was not measured. Never report it as zero.
- Dependencies in a report from `wrap` or an imported trace are inferred, so say so.

## More detail

Read `references/effects.md` when you need what each effect tier allows.
