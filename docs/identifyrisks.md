
# Using LLMs to Surface Hidden Architectural Risks

| Stage                        | Traditional Pain                                                       | How an LLM Helps                                                                                                                                                                      | Quick Win                                                                        |
| ---------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Design Docs / Diagrams**   | Manual review misses edge‑cases; reviewers focus on “happy path.”      | *Feed the diagram / markdown into an LLM prompt:* “List scaling, coupling, or resiliency risks.” <br>*Model spots tight sync calls, single‑AZ deployments, missing circuit breakers.* | **Design‑Risk GPT** prompt template that every architect runs before ARB.               |
| **Pull / Merge Requests**    | Human reviewers skim; deep latency or retry issues slip through.       | *Fine‑tune or few‑shot an LLM on past post‑mortems.* <br>*Ask:* “Does this code add any unbounded loops, N+1 queries, large payloads?”                                                | Add an **LLM gate in CI** that comments on PRs with flagged smells and guardrail links. |
| **Infra‑as‑Code (IaC)**      | Misconfigs (open security groups, no autoscaling) appear only in prod. | *Parse Terraform / CloudFormation; prompt:* “Find resources without autoscaling / multi‑AZ / tagging.”                                                                                | Nightly **LLM lint** on IaC repos; fail builds on critical patterns.                    |
| **Runtime Telemetry & Logs** | SREs drown in metrics; relationships between spikes are unclear.       | *Stream logs/metrics to embeddings store.* <br>*Query:* “What new anomaly clusters appeared last 30 min?” <br>*LLM correlates latency spike with deployment & queue depth.*           | Batch job: dump yesterday’s logs ➜ **LLM summary** emailed to on‑call.                  |
| **Traffic Pattern Analysis** | Sudden usage skew causes hot partitions.                               | *Feed trace stats:* “Detect endpoints whose P95 grew >x but traffic change \<y.” <br>*LLM suggests shard‑by‑tenant or caching.*                                                       | Weekly cron: trace stats → LLM → auto‑create incidents if anomaly > threshold.               |

---

## Implementation Tips

1. **Start small & offline** – run on saved artifacts, refine prompts, then automate.
2. **Ground the model** – supply guidelines, past incidents, SLO docs as context.
3. **Treat output as review, not truth** – engineers confirm/deny; feedback improves prompts.
4. **Close the loop** – when a risk causes an incident, add it to exemplar set so the LLM learns.

---

## Example Prompt for a PR Check

```text
SYSTEM: You are a scaling‑risk reviewer.
INPUT: <diff snippet>
CONTEXT: • Max query time 200 ms
          • External calls must be async with timeouts
TASK: Identify scaling/resiliency risks, cite lines, suggest fixes.
```
