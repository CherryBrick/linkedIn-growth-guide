# Parameterization Schema

This document codifies the universal, base input parameters that drive the LinkedIn growth pipeline. The parameters are declared at pipeline initialization, validated against a concise set of business rules, and then used to tailor tactic selection, messaging, content strategy, and risk controls. The schema is designed to be machine-readable and extensible, with explicit data types, allowed values, defaults, and documentation of how each parameter feeds into downstream pipeline steps.

Introduction
- The LinkedIn growth pipeline operates on a fixed set of base inputs. These inputs are intentionally expressive enough to cover target audience definition, outreach behavior, content strategy, and regulatory considerations, yet constrained by validation rules to avoid inconsistent configurations.
- Values often derive from Pareto analyses and evidence from the synthesis base (see synth-pareto.md, Section 7). Defaults favor safe, high-ROI settings.
- Derived variables compute secondary controls from the base inputs, providing guardrails that keep activity within platform terms and risk budgets.

Quick-reference (base parameters)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| target_industry | String (enum) | tech | Primary industry of the target audience; shapes pillars, sources, and timing. |
| target_persona | String (enum) | individual_contributor | Role of the ideal connection; drives messaging tone and content alignment. |
| geography | String (ISO code) | US | Geographic focus for networking; influences language, timezone, and acceptance patterns. |
| language | String (ISO code) | en | Primary language for content and outreach; should match audience. |
| seniority_level | String (enum) | mid | Target connection seniority; affects messaging formality and depth. |
| goal_type | String (enum) | connections | Primary objective of LinkedIn activity (e.g., connections, leads, hires). |
| risk_tolerance | String (enum) | conservative | Willingness to accept higher-risk tactics; drives tactic filtering and automation limits. |
| automation_level | String (enum) | manual | Degree of automation allowed; trades off speed against compliance risks. |
| content_tone | String (enum) | professional | Tone for content and outreach. |
| connection_daily_limit | Integer | 15 | Max connection requests per day; controls pacing and risk of platform-related limits. |
| message_personalization_depth | String (enum) | contextual | Depth of personalization for outreach messages. |
| current_connection_count | Integer | 0 | Current first-degree connections; informs weekly limits and growth velocity. |
| current_ssi_score | Integer | 0 | Social Selling Index; influences limits and outreach effectiveness. |
| content_capacity | Integer (hours/week) | 5 | Hours per week available for content creation and engagement. |
| budget_monthly | Integer (USD) | 0 | Monthly budget for tools and paid features. |
| brand_voice | String (enum) | professional | Desired voice for content and outreach. |
| key_value_proposition | String | "" | Core value proposition communicated in profile and messaging. |
| time_to_goal_weeks | Integer | 12 | Target timeline to reach the primary objective. |
| existing_audience_size | Integer | 0 | Current follower base; informs strategy focus. |
| previous_restrictions | Boolean | false | Indicates prior LinkedIn restrictions; triggers higher caution. |
| team_size | Integer | 1 | Number of people executing the plan; affects collaboration tactics. |
| data_compliance_region | String (enum) | none | Compliance region for data handling (e.g., gdpr, ccpa, none). |

Notes:
- The table above is a compact quick-reference. Each parameter is elaborated in the sections below with data types, allowed values, defaults, and which pipeline steps consume the parameter.

Derived variables (computed from base parameters)
- safe_weekly_connections: MIN(80, IF(previous_restrictions, 30, IF(current_ssi_score > 70, 150, IF(current_ssi_score > 40, 100, 50))))
  - Description: Upper bound on weekly connection requests, balancing health of the account and growth goals. Used by outreach scheduling and limit enforcement.
- safe_daily_messages: IF(current_ssi_score > 70, 30, IF(current_ssi_score > 40, 20, 15))
  - Description: Maximum safe messages per day to avoid alarming the platform and triggering anti-spam signals.
- recommended_posting_frequency: MIN(7, MAX(2, ROUND(content_capacity / 2.5)))
  - Description: Suggested number of posts per week given available content capacity.
- tactic_eligibility: FILTER(tactics, risk <= risk_tolerance AND automation_req <= automation_level)
  - Description: Subset of techniques allowed by risk and automation preferences.

Validation rules (cross-parameter constraints)
- Risk vs automation: Conservative risk tolerance implies manual or semi-automation modes; aggressive risk tolerance should allow semi or automated automation with stronger safeguards.
- Geographic and language alignment: language must be compatible with geography where possible (e.g., en in US/UK; es in Spain/LatAm) to preserve coherence and compliance.
- Previous restrictions increase caution: If previous_restrictions is true, lower daily and weekly limits become mandatory until recovery is proven.
- Data compliance region governs permitted data enrichment and scraping activities; tools and features requiring data transfer must be constrained accordingly.
- Time-to-goal alignment: Shorter time horizons require higher content capacity and automation, with tighter validation around risk and compliance.

Derived variable formulas and pipeline usage notes (extended)
- Each derived variable is computed during pipeline initialization or refresh and then used to configure downstream modules:
  - safe_weekly_connections drives outreach pacing and weekly limit enforcement in the outreach module.
  - safe_daily_messages feeds the message sequencing and daily outreach quotas.
  - recommended_posting_frequency informs the content calendar and posting schedule generator.
  - tactic_eligibility feeds the tactic selector component, shaping which tactics are proposed given risk and automation preferences.

Source citations
- Primary source: Section 8.2 (Required Parameters) of the Parameterization Schema in linkedin-growth-research/docs/08-parameterization-schema.md.
- Design principles (8.1) and Derived Variables (8.3) provide the rationale for defaults, validation, and computed fields.
- Appendix references: Cross-reference to Appendix B: Parameterization Quick Reference in the original document.

Appendix B: Parameterization Quick Reference ( cross-reference )
- See the original Appendix B in the Parameterization Schema for a compact reference table aligned with the fields above.

Appendix: Data sources and provenance
- The parameter definitions mirror the enumerated values present in the Section 8 parameter table, including the exact enumerations and default values as documented.

Source citations
- 8.1 Schema Design Principles
- 8.2 Required Parameters
- 8.3 Derived Variables
- synth-pareto.md (Section 7) for defaults reference

Notes for implementers
- This document is intended to be the authoritative schema for all downstream components. Any additions should maintain backward compatibility and be validated against the cross-parameter rules described above.

Source: LinkedIn Growth Research – Parameterization Schema, Section 8 (Section 8.1–8.3) in docs/08-parameterization-schema.md
