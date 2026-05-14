## Section 8: Parameterization Schema

## Parameterization Schema (08-parameterization-schema):

## Section (Modified for readability)

1. **Evidence-Driven Defaults:** Default values are set to the safest, highest-ROI settings identified in the Pareto analysis (Section 7 of synth-pareto.md).

2. **Validation Rules:** Cross-parameter constraints enforce logical consistency (e.g., conservative risk tolerance forces manual automation level).

3. **Derived Variables:** Computed parameters reduce redundant inputs and enforce evidence-based limits (e.g., safe weekly connections based on SSI score).


### 8.3 Derived Variables (Computed from Base Parameters)

| Variable Name | Formula / Derivation | Description | Pipeline Step(s) |
|---------------|----------------------|-------------|-----------------|
| `safe_weekly_connections` | MIN(80, IF(`previous_restrictions`, 30, IF(`current_ssi_score` > 70, 150, IF(`current_ssi_score` > 40, 100, 50)))) | Maximum safe weekly connection requests based on account health and SSI. | Outreach scheduling, Limit enforcement |
| `safe_daily_messages` | IF(`current_ssi_score` > 70, 30, IF(`current_ssi_score` > 40, 20, 15)) | Maximum safe daily messages to connections. | Message sequencing, Outreach volume |
| `recommended_posting_frequency` | MIN(7, MAX(2, ROUND(`content_capacity` / 2.5))) | Recommended posts per week based on available time. | Content calendar, Posting schedule |
| `tactic_eligibility` | FILTER(tactics, risk <= `risk_tolerance` AND automation_req <= `automation_level`) | Subset of tactics that match the account's risk and automation preferences. | Strategy generation, Tactic recommendation |
| `estimated_connections_per_week` | `safe_weekly_connections` * acceptance_rate(`target_persona`, `seniority_level`, `message_personalization_depth`) | Projected weekly connection growth based on safe limits and historical acceptance rates for the target segment. | Milestone planning, Goal feasibility |
| `phase` | IF(`current_connection_count` < 500, "foundation", IF(`current_connection_count` < 2000, "growth", IF(`current_connection_count` < 10000, "scale", "optimize"))) | Current growth phase based on connection count. Determines which tactics to prioritize. | Strategy selection, Milestone definition |


### 8.5 Parameter Impact on Pipeline Execution

The following table shows how each parameter affects the four core pipeline stages:

| Parameter | Profile Optimization | Content Strategy | Outreach & Messaging | Risk & Compliance |
|-----------|---------------------|------------------|---------------------|-------------------|
| `target_industry` | HIGH | HIGH | HIGH | LOW |
| `target_persona` | HIGH | HIGH | HIGH | LOW |
| `geography` | MEDIUM | HIGH | HIGH | LOW |
| `language` | HIGH | HIGH | HIGH | LOW |
| `seniority_level` | HIGH | MEDIUM | HIGH | LOW |
| `goal_type` | MEDIUM | HIGH | HIGH | MEDIUM |
| `risk_tolerance` | LOW | LOW | MEDIUM | HIGH |
| `automation_level` | LOW | LOW | HIGH | HIGH |
| `content_tone` | LOW | HIGH | MEDIUM | LOW |
| `connection_daily_limit` | LOW | LOW | HIGH | HIGH |
| `message_personalization_depth` | LOW | LOW | HIGH | LOW |
| `current_ssi_score` | HIGH | LOW | HIGH | HIGH |
| `budget_monthly` | MEDIUM | MEDIUM | HIGH | MEDIUM |
| `time_to_goal_weeks` | LOW | MEDIUM | HIGH | MEDIUM |
| `previous_restrictions` | LOW | LOW | MEDIUM | HIGH |

---



> **Cross-Reference:** For a quick-reference table of all parameters described in this schema, see [Appendix B: Parameterization Quick Reference](#appendix-b-parameterization-quick-reference).