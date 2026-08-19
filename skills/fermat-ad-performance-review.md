---
name: fermat-ad-performance-review
description: Review paid-media performance across Meta, TikTok and Google in a FERMÀT account — spend, clicks, ROAS and CPA by ad and by destination — and hand back the creatives worth cutting or scaling.
api: FERMAT Platform MCP
endpoint: https://mcp.fermatcommerce.com/mcp/fermat-mcp
operations:
  - get_ad_insights
  - get_ad_creatives
  - get_destination_insights
  - get_destination_details
  - ask_pierre
generated: '2026-08-13'
method: generated
source: mcp/fermat-mcp.yml (tool names verified against the provider's public install manifest)
---

# Ad performance review

Read-only. Every tool below is a real, published FERMÀT MCP tool.

## Before you start
- Connect the server at `https://mcp.fermatcommerce.com/mcp/fermat-mcp` and complete the OAuth
  authorization-code + PKCE flow. An unauthenticated call returns JSON-RPC `-32001`.
- Access is scoped per-brand. Confirm which organization you are operating on before reporting
  numbers — several tools take an organization/shop identifier.

## Steps
1. **Pull ad-level metrics.** Call `get_ad_insights` for the reporting window. This is the source
   for spend, clicks, ROAS and CPA per ad across Meta, TikTok and Google.
2. **Attach the creative.** Call `get_ad_creatives` so each metric row is tied to the asset a human
   can recognize. Do not report an ad ID without its creative.
3. **Split by destination.** Call `get_destination_insights` for per-channel ROAS, revenue and
   attribution, and `get_destination_details` for any destination that looks anomalous. An ad that
   underperforms on one destination and wins on another is a routing finding, not a creative one.
4. **Ask Pierre for the read.** `ask_pierre` is FERMÀT's built-in BI agent — use it for the
   strategic framing ("what is my biggest growth opportunity"), not as a substitute for the
   metric tools above. Treat its answer as narrative and the tool output as evidence.

## Rules
- **Read-only.** External MCP access cannot modify a funnel, offer or campaign. If a request
  implies a write, say so and stop.
- **No published rate limits and no rate-limit headers.** Batch requests conservatively and do not
  build a tight polling loop — you will get no runtime signal before you are cut off.
- Attribution changed materially on 2026-07-03 (order attribution moved from ~25% to near 100%,
  per the changelog). Do not compare windows that straddle that date without flagging it.
- Errors are JSON-RPC 2.0, not RFC 9457 — see `errors/fermat-problem-types.yml`.
