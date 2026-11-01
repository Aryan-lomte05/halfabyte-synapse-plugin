# Effect tiers

Every step has one effect. An unknown tool is treated as `external`.

| Effect | Meaning | Example | Allowed |
|---|---|---|---|
| `pure` | Same input, same output, nothing observable | Formatting text | Everything |
| `read` | Observes outside state | Web search | Cache within `staleness_s`, reorder among reads, run in parallel, batch |
| `write` | Changes workflow state | Updating a cart | Run in parallel only on different keys |
| `external` | Changes the outside world | Sending an email | Nothing. Stubbed outside a live run |
| `irreversible` | Cannot be undone, or moves money | Placing an order | Nothing. Never run during search, verification or benchmarks |

## Determinism

`stochastic` steps are not collapsed or cached, because merging two samples changes the output
distribution. `seeded` and `deterministic` steps can be.

## Edges

- **Data edge:** one step needs another's result. Having the value already removes the wait.
- **Effect edge:** this order must not change. No pass may break it, and the scheduler checks
  again at run time.

Two steps with no path between them may run at the same time.
