---
section: 8
title: Parameterization Schema
word_count: 1539
---

## Section 8: Parameterization Schema

This section defines the universal input parameters for the LinkedIn growth pipeline. All parameters are collected at pipeline initialization and used to personalize tactic selection, messaging, content strategy, and risk thresholds. The schema is designed to be machine-readable and validated against the evidence base.

### 8.1 Schema Design Principles

1. **Evidence-Driven Defaults:** Default values are set to the safest, highest-ROI settings identified in the Pareto analysis (Section 7 of synth-pareto.md).

2. **Validation Rules:** Cross-parameter constraints enforce logical consistency (e.g., conservative risk tolerance forces manual automation level).

3. **Derived Variables:** Computed parameters reduce redundant inputs and enforce evidence-based limits (e.g., safe weekly connections based on SSI score).

---

### 8.2 Required Parameters

| Parameter Name | Data Type | Valid Values / Range | Default Value | Description | Pipeline Steps Using This Parameter |
|----------------|-----------|----------------------|---------------|-------------|-------------------------------------|
| `target_industry` | String (enum) | tech, finance, healthcare, manufacturing, consulting, education, media, retail, other | "tech" | Primary industry of the target audience. Determines content pillars, connection sources, and optimal posting times. | Profile optimization, Content strategy, Outreach targeting, Connection filters |
| `target_persona` | String (enum) | founder, executive, manager, individual_contributor, recruiter, investor, freelancer, consultant | "individual_contributor" | Role of the ideal connection. Influences messaging tone, value proposition, and engagement strategy. | Messaging personalization, Content topics, Outreach sequence design |
| `geography` | String (ISO code) | Any valid ISO-3166 country code or region ("US", "EU", "APAC", "global") | "US" | Geographic focus of networking efforts. Impacts language, cultural norms, posting timezone, and connection acceptance patterns. | Post timing, Language selection, Outreach targeting, Event planning |
| `language` | String (ISO code) | en, es, de, fr, pt, zh, ja, ko, other | "en" | Primary language for content and outreach. Should match target audience's preferred language. | Content creation, Message drafting, Profile optimization |
| `seniority_level` | String (enum) | entry, mid, senior, director, vp, c_suite | "mid" | Seniority of target connections. Affects messaging formality, content depth, and connection request approach. | Outreach tone, Content complexity, Sales Navigator filters |
| `goal_type` | String (enum) | connections, leads, sales, brand, hiring, partnerships, investment | "connections" | Primary objective of LinkedIn activity. Determines KPIs, content mix, and outreach aggressiveness. | Strategy selection, KPI definition, Content mix, Outreach CTA |
| `risk_tolerance` | String (enum) | conservative, moderate, aggressive | "conservative" | Willingness to use gray-area or ToS-violating tactics. Conservative = organic only; Moderate = some compliant tools; Aggressive = accepts automation risk. | Tactic filtering, Tool selection, Connection rate limits |
| `automation_level` | String (enum) | manual, semi, automated | "manual" | Degree of automation acceptable. Manual = all actions human-performed; Semi = AI-assisted drafting, human-approved send; Automated = full automation (ToS-violating). | Tool selection, Workflow design, Compliance checks |
| `content_tone` | String (enum) | professional, casual, contrarian, inspirational, educational | "professional" | Tone of voice for content creation. Should align with target persona expectations and industry norms. | Content creation, Message drafting, Profile copy |
| `connection_daily_limit` | Integer | 5 - 50 | 15 | Maximum connection requests per day. Must stay under 80/week to avoid 2026 spam filters. Free accounts: 20-30/day; Premium: 30-50/day; Sales Navigator: up to 75/day. | Outreach scheduling, Limit enforcement, Daily task generation |
| `message_personalization_depth` | String (enum) | basic, contextual, hyper | "contextual" | Level of personalization for outreach messages. Basic = name + company; Contextual = reference recent activity; Hyper = multiple signals + custom research. | Message generation, Connection request notes, Follow-up sequences |
| `current_connection_count` | Integer | 0 - 30000 | 0 | Current number of 1st-degree connections. Determines weekly safe limits, account maturity phase, and growth velocity. | Limit calculation, Phase planning, SSI baseline |
| `current_ssi_score` | Integer | 0 - 100 | 0 | Current Social Selling Index. Influences connection request limits, algorithmic favorability, and outreach success rates. | Limit calculation, Strategy prioritization, Benchmarking |
| `content_capacity` | Integer (hours/week) | 1 - 40 | 5 | Hours per week available for content creation and engagement. Determines posting frequency, comment volume, and content format complexity. | Content calendar design, Posting frequency, Engagement targets |
| `budget_monthly` | Integer (USD) | 0 - 10000 | 0 | Monthly budget for tools, subscriptions, and paid features. Determines access to Sales Navigator, automation tools, and enrichment services. | Tool selection, Subscription recommendations, Paid feature access |
| `brand_voice` | String (enum) | professional, casual, authoritative, humorous, inspirational, contrarian | "professional" | Desired tone of voice for content and outreach. Should align with target persona expectations and industry norms. | Content creation, Message drafting, Profile copy |
| `key_value_proposition` | String | Free text, 10-200 characters | "" | Core value proposition to communicate in profile, content, and outreach. Should be specific and outcome-oriented. | Profile optimization, Messaging personalization, Content hooks |
| `time_to_goal_weeks` | Integer | 1 - 52 | 12 | Desired timeline to reach the primary goal. Determines tactic aggressiveness, posting frequency, and resource allocation. | Phase planning, Tactic selection, Milestone definition |
| `existing_audience_size` | Integer | 0 - 1000000 | 0 | Current follower count (not connections). Determines whether to prioritize growth or monetization/engagement. | Strategy phase, Content distribution approach, Monetization readiness |
| `previous_restrictions` | Boolean | true, false | false | Whether the account has previously received LinkedIn restrictions. Dramatically increases caution requirements and reduces safe limits. | Risk assessment, Limit calculation, Recovery protocol |
| `team_size` | Integer | 1 - 1000 | 1 | Number of people who will be executing the strategy. Determines whether employee advocacy, team link, and collaborative tactics are viable. | Strategy scope, Tool selection (team features), Advocacy potential |
| `data_compliance_region` | String (enum) | gdpr, ccpa, none, other | "none" | Data privacy compliance requirements. Affects data enrichment, scraping, and storage practices. | Tool selection, Data handling, Scraping permissions |

---

### 8.3 Derived Variables (Computed from Base Parameters)

| Variable Name | Formula / Derivation | Description | Pipeline Step(s) |
|---------------|----------------------|-------------|-----------------|
| `safe_weekly_connections` | MIN(80, IF(`previous_restrictions`, 30, IF(`current_ssi_score` > 70, 150, IF(`current_ssi_score` > 40, 100, 50)))) | Maximum safe weekly connection requests based on account health and SSI. | Outreach scheduling, Limit enforcement |
| `safe_daily_messages` | IF(`current_ssi_score` > 70, 30, IF(`current_ssi_score` > 40, 20, 15)) | Maximum safe daily messages to connections. | Message sequencing, Outreach volume |
| `recommended_posting_frequency` | MIN(7, MAX(2, ROUND(`content_capacity` / 2.5))) | Recommended posts per week based on available time. | Content calendar, Posting schedule |
| `tactic_eligibility` | FILTER(tactics, risk <= `risk_tolerance` AND automation_req <= `automation_level`) | Subset of tactics that match the account's risk and automation preferences. | Strategy generation, Tactic recommendation |
| `estimated_connections_per_week` | `safe_weekly_connections` * acceptance_rate(`target_persona`, `seniority_level`, `message_personalization_depth`) | Projected weekly connection growth based on safe limits and historical acceptance rates for the target segment. | Milestone planning, Goal feasibility |
| `phase` | IF(`current_connection_count` < 500, "foundation", IF(`current_connection_count` < 2000, "growth", IF(`current_connection_count` < 10000, "scale", "optimize"))) | Current growth phase based on connection count. Determines which tactics to prioritize. | Strategy selection, Milestone definition |

---

### 8.4 Schema Validation Rules

1. **Industry-Persona Compatibility:** `target_industry` and `target_persona` must be compatible (e.g., "founder" persona is valid across all industries, but "recruiter" may have limited relevance in some contexts).

2. **Risk-Automation Lock:** `risk_tolerance` = "conservative" forces `automation_level` = "manual" and excludes all ToS-violating tactics.

3. **Restriction Penalty:** `previous_restrictions` = true forces `safe_weekly_connections` to 30 regardless of SSI score.

4. **Budget Gate:** `budget_monthly` < 100 excludes Sales Navigator and all paid automation tools.

5. **Time Pressure:** `time_to_goal_weeks` < 4 forces `automation_level` to "manual" or "semi" (no time to recover from restrictions).

6. **Connection Cap:** `current_connection_count` >= 30000 triggers follower-first strategy (cannot add more connections).

7. **Content Capacity Floor:** `content_capacity` < 2 hours/week forces `recommended_posting_frequency` to 1 post/week minimum.

8. **Language-Geography Sync:** `language` should match the primary language of `geography` (warning if mismatch detected).

---

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

