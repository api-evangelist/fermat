---
name: fermat-friction-and-anomaly-triage
description: Triage what is going wrong on a FERMÀT storefront — auto-detected anomalies with root cause, UX friction points, rage and dead clicks, scroll depth and checkout drop-off — and rank the fixes.
api: FERMAT Platform MCP
endpoint: https://mcp.fermatcommerce.com/mcp/fermat-mcp
operations:
  - get_anomalies
  - get_anomaly_report
  - get_friction_point_report
  - get_heatmap_click_analysis
  - get_heatmap_scroll_analysis
  - get_session_checkout_analysis
  - get_session_behavioral_insights
  - get_session_funnel_journey
generated: '2026-08-13'
method: generated
source: mcp/fermat-mcp.yml (tool names verified against the provider's public install manifest)
---

# Friction and anomaly triage

Read-only. Every tool below is a real, published FERMÀT MCP tool.

## Steps
1. **Start with what FERMÀT already flagged.** `get_anomalies` lists auto-detected anomalies;
   `get_anomaly_report` carries the root-cause analysis. Lead with these — they are the platform's
   own detections, not your inference.
2. **Get the ranked friction feed.** `get_friction_point_report` returns automatically detected UX
   issues. Per the 2026-07-09 changelog these are ranked and carry session-replay links; surface
   the link so a human can watch the failure rather than read about it.
3. **Localize it on the page.** `get_heatmap_click_analysis` for rage and dead clicks,
   `get_heatmap_scroll_analysis` for depth. A friction point without a location on the page is not
   yet a fix.
4. **Follow the session.** `get_session_funnel_journey` for the path, `get_session_behavioral_insights`
   for behavioral patterns and intention scoring, and `get_session_checkout_analysis` when the loss
   is at checkout specifically.
5. **Rank by money, not by count.** Cross-reference against `get_funnel_metrics` (see the funnel
   readout skill) so the ranking reflects revenue at risk rather than raw event volume.

## Rules
- **Read-only.** You can diagnose but you cannot change a layout, module or funnel.
- Session data is behavioral and shopper-level. Report it in aggregate; do not extract or restate
  individual shopper identifiers.
- Requires an active FERMÀT account with the pixel installed — sessions only exist where FERMÀT
  Pixel v2 is deployed (`components/fermat-components.yml`).
