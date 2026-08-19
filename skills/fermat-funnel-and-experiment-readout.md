---
name: fermat-funnel-and-experiment-readout
description: Produce a funnel performance and A/B experiment readout from a FERMÀT account — sessions, conversions, revenue, CVR and AOV, plus which variants are winning and whether the result is significant.
api: FERMAT Platform MCP
endpoint: https://mcp.fermatcommerce.com/mcp/fermat-mcp
operations:
  - list_funnels
  - get_funnel_metrics
  - list_experiments
  - get_experiment_results
  - list_active_dpp_experiments
  - list_pdp_versions
generated: '2026-08-13'
method: generated
source: mcp/fermat-mcp.yml (tool names verified against the provider's public install manifest)
---

# Funnel and experiment readout

Read-only. Every tool below is a real, published FERMÀT MCP tool.

## Steps
1. **Enumerate the funnels.** `list_funnels` gives the surfaces in scope. Never assume a funnel ID.
2. **Pull the metrics.** `get_funnel_metrics` returns sessions, conversions, revenue, CVR and AOV
   over a date range. Pull the comparison window in the same call pattern so the periods match.
3. **Enumerate experiments.** `list_experiments` for the full set; `list_active_dpp_experiments`
   for what is live on dynamic product pages right now.
4. **Read the results.** `get_experiment_results` carries A/B outcomes, routing splits and product
   group experiments **with statistical significance**. Report the significance alongside the lift
   — a variant that is ahead but not significant is not a winner, and must not be described as one.
5. **Tie a winning variant to a layout.** `list_pdp_versions` maps the winning arm back to the
   product-page version that produced it, so the result is actionable rather than abstract.

## Rules
- **Read-only.** You cannot promote a variant or end an experiment through the MCP server.
- Report the date range explicitly on every number. `get_funnel_metrics` is range-scoped and a
  bare figure is meaningless without it.
- If an experiment has no significance value, say the data does not support a call.
