## Section 7: Prompt Surface Map

## Prompt Surface Map (07-prompt-surface-map):

## Section (Modified for readability)

The prompt surface follows three design principles derived from the evidence base:

1. **Context-Rich Inputs:** The 2026 LinkedIn algorithm ("360 Brew") uses LLMs to evaluate content quality and profile authority [S072]. Prompts must therefore supply sufficient context about the user's profile, target audience, and strategic goals to produce outputs that satisfy algorithmic evaluation.

2. **Structured Output Formats:** LinkedIn's distribution system rewards "frameworks, practical checklists, specific guidance — things people bookmark" [S055]. Prompts should enforce structured output formats (JSON, markdown tables, numbered lists) that align with high-performing content patterns.

3. **Chain-of-Thought for Complex Tasks:** Multi-step sequences (outreach campaigns, content calendars) benefit from chain-of-thought reasoning to maintain coherence across outputs [S058]. Simple tasks (headline generation) can use few-shot prompting.


### 7.3 Prompt Point 2: Profile About Section Generation

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Profile About Section Generation |
| **Input Variables** | `target_industry`, `target_persona`, `key_value_proposition`, `seniority_level`, `brand_voice`, `competitor_accounts`, `existing_audience_size` |
| **Output Format** | Full About section (500-800 characters) + PAS framework breakdown + keyword list |
| **Suggested Strategy** | Chain-of-thought with structured sections |
| **Parameters Affected** | `target_industry`, `target_persona`, `key_value_proposition`, `brand_voice`, `competitor_accounts` |

**Example Prompt Template:**

```
Write a LinkedIn About section that functions as a conversion tool, not a biography.

CONTEXT:
- Industry: {target_industry}
- Target persona: {target_persona}
- Value proposition: {key_value_proposition}
- Seniority: {seniority_level}
- Tone: {brand_voice}

REQUIREMENTS:
- 500-800 characters
- PAS framework: Problem (2 lines) + Agitate (3-5 lines) + Solution (3-5 lines)
- Include social proof and a clear CTA
- Embed primary keywords naturally in the first 50-70 words (LLM citation patterns favor early text)
- Ensure semantic coherence with headline and experience sections

OUTPUT FORMAT:
Return a JSON object with:
- "about_text": string (full text)
- "pas_breakdown": { "problem": string, "agitate": string, "solution": string }
- "cta": string
- "keywords": array of strings
- "character_count": integer

EVIDENCE: Profiles with precisely defined About sections are "up to 40x more discoverable in opportunity searches" (Goodman, 2025). The 2026 algorithm checks headline, About, and Experience for topical authority before distributing content (Winter, 2026).
```

**Source Citations:** [S079] Melanie Goodman (2025-09-08) — 40x discoverability; [S072] Sean Winter (2026-03-27) — cross-section authority checking; [S085] Melanie Goodman (2026-01-21) — PAS framework recommendation.


### 7.5 Prompt Point 4: Follow-Up Message Sequence Generation

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Follow-Up Message Sequence Generation |
| **Input Variables** | Prospect profile data, connection context, `goal_type`, `risk_tolerance`, `automation_level`, `brand_voice`, `message_personalization_depth` |
| **Output Format** | 3-5 step message sequence with timing, content, and CTAs for each step |
| **Suggested Strategy** | Chain-of-thought with sequence logic |
| **Parameters Affected** | `goal_type`, `risk_tolerance`, `automation_level`, `brand_voice`, `message_personalization_depth` |

**Example Prompt Template:**

```
Generate a multi-step LinkedIn outreach sequence for {prospect_name} who accepted a connection request on {connection_date}.

CONTEXT:
- Goal type: {goal_type}
- Risk tolerance: {risk_tolerance}
- Automation level: {automation_level}
- Tone: {brand_voice}
- Personalization depth: {message_personalization_depth}
- Prospect: {prospect_name}, {prospect_title}, {prospect_company}
- Initial connection context: {connection_context}

SEQUENCE REQUIREMENTS:
- 3-5 touches over 14-30 days
- Day 0: Connection request (already sent)
- Day 3-4: Value-first follow-up (no pitch)
- Day 7-10: Soft CTA or resource share
- Day 14-30: Final follow-up or breakup message
- Decreasing aggressiveness after each step
- Stop after 4-5 touches with no response

OUTPUT FORMAT:
Return a JSON object with:
- "sequence": array of objects, each with { "day": integer, "message": string, "cta": string, "purpose": string }
- "timing_notes": string (best practices for this prospect type)
- "stop_condition": string (when to stop following up)

EVIDENCE: Multi-step outreach sequences consistently outperform single-touch approaches. The optimal sequence length is 3-5 touches over 14-30 days (Akram, 2026).
```

**Source Citations:** [S065] Shahzad Akram (2026-03-13) — sequence frameworks; [S058] Talha Fakhar (2026-03-13) — follow-up timing and cadence; [S066] Inbound Agency Dubai (2026-01-23) — 3-step sequence structure.


### 7.7 Prompt Point 6: Content Draft Writing

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Content Draft Writing |
| **Input Variables** | Content idea, `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `language`, `key_value_proposition`, `geography` |
| **Output Format** | Full post draft (800-1000 words for text, 8-10 slides for carousel) + hook + CTA |
| **Suggested Strategy** | Chain-of-thought with structural constraints |
| **Parameters Affected** | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `language`, `key_value_proposition` |

**Example Prompt Template:**

```
Write a LinkedIn post draft based on the following idea.

IDEA: {content_idea}

CONTEXT:
- Industry: {target_industry}
- Target persona: {target_persona}
- Tone: {brand_voice}
- Content tone: {content_tone}
- Language: {language}
- Value proposition: {key_value_proposition}

REQUIREMENTS:
- 800-1000 words (performs 26% better than shorter posts)
- First 50-70 words must contain the direct answer/hook (44.2% of LLM citations come from first 30% of text)
- Structure: Hook → Problem → Solution → Framework → CTA
- Include a numbered framework or checklist (maximizes saves)
- Zero-click content: give full value in the post, no external links
- End with a soft CTA: "Comment 'guide' and I'll send it" or "If you want, I'll do this teardown on your setup"
- Use line breaks for readability (mobile-first)

OUTPUT FORMAT:
Return a JSON object with:
- "post_text": string (full draft)
- "hook": string (first 50-70 words)
- "framework": string (numbered list or checklist)
- "cta": string
- "word_count": integer
- "suggested_hashtags": array of strings (3-5 max, though hashtags don't matter for distribution in 2026)

EVIDENCE: Posts between 800-1000 words perform 26% better. The 2026 "360 Brew" algorithm uses LLMs to evaluate content quality, rewarding "frameworks, practical checklists, specific guidance" (Winter, 2026).
```

**Source Citations:** [S072] Sean Winter (2026-03-27) — 360 Brew algorithm; [S055] Frank Furness (2026-01-30) — word count benchmarks; [S052] Elizabeta (2026-02-24) — LLM citation patterns.


### 7.9 Prompt Point 8: Lead Qualification Scoring

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Lead Qualification Scoring |
| **Input Variables** | Prospect profile data (title, company, industry, activity, mutual connections), `target_industry`, `target_persona`, `seniority_level`, `goal_type` |
| **Output Format** | Qualification score (0-100) + qualification tier + recommended next action |
| **Suggested Strategy** | Structured output with weighted scoring rubric |
| **Parameters Affected** | `target_industry`, `target_persona`, `seniority_level`, `goal_type` |

**Example Prompt Template:**

```
Score and qualify a LinkedIn prospect based on their profile and activity.

PROSPECT DATA:
- Name: {prospect_name}
- Title: {prospect_title}
- Company: {prospect_company}
- Industry: {prospect_industry}
- Company size: {company_size}
- Recent activity: {recent_activity}
- Mutual connections: {mutual_count}
- Connection degree: {degree}

TARGET CRITERIA:
- Target industry: {target_industry}
- Target persona: {target_persona}
- Target seniority: {seniority_level}
- Goal type: {goal_type}

SCORING RUBRIC (0-100):
- Title/Role match (25 points)
- Industry match (20 points)
- Company size fit (15 points)
- Activity level (15 points)
- Mutual connections (10 points)
- Content engagement (10 points)
- Recent job change (5 points bonus)

OUTPUT FORMAT:
Return a JSON object with:
- "total_score": integer (0-100)
- "tier": string ("hot" >=80, "warm" 60-79, "cold" <60)
- "score_breakdown": object with individual scores
- "recommended_action": string ("connect_now", "engage_first", "nurture", "disqualify")
- "personalization_angle": string (best angle for outreach)

EVIDENCE: Warm DM protocol (engaging twice in comments before connecting) achieves 3-4x higher response rates than cold outreach (Freligh, 2026).
```

**Source Citations:** [S050] Tai Freligh (2026-02-05) — warm DM protocol; [S064] Zaidan Ahmad (2025-11-08) — LinkedIn vs. cold email performance.


### 7.11 Prompt Point 10: Weekly Growth Report Summary

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Weekly Growth Report Summary |
| **Input Variables** | Weekly metrics (connections, profile views, post impressions, saves, comments, SSI), `goal_type`, `time_to_goal_weeks`, `phase` |
| **Output Format** | Narrative summary + tactical recommendations + alert flags |
| **Suggested Strategy** | Structured output with data interpretation rules |
| **Parameters Affected** | `goal_type`, `time_to_goal_weeks`, `phase`, `content_capacity` |

**Example Prompt Template:**

```
Generate a weekly LinkedIn growth report summary and recommendations.

WEEKLY METRICS:
- New connections: {new_connections}
- Profile views: {profile_views}
- Post impressions: {post_impressions}
- Post saves: {post_saves}
- Comments received: {comments_received}
- Comments given: {comments_given}
- SSI score: {ssi_score}
- Connection acceptance rate: {acceptance_rate}

CONTEXT:
- Goal type: {goal_type}
- Current phase: {phase}
- Weeks to goal: {time_to_goal_weeks}
- Content capacity: {content_capacity} hours/week

REPORT REQUIREMENTS:
- Narrative summary of week's performance vs. benchmarks
- Identification of top-performing content/tactic
- Alert flags for concerning trends (e.g., acceptance rate <20%, SSI decline)
- 3 specific actions for next week
- Milestone progress assessment

OUTPUT FORMAT:
Return a JSON object with:
- "summary": string (2-3 paragraph narrative)
- "benchmarks": object (metrics vs. phase-appropriate benchmarks)
- "top_performer": string (best content or tactic this week)
- "alerts": array of strings (warning flags)
- "recommendations": array of 3 strings (specific next actions)
- "milestone_progress": string (% toward next milestone)
- "phase_assessment": string (on_track, ahead, behind)

EVIDENCE: Members posting twice per week see up to 5x more profile views (Furness, 2026). SSI 70+ unlocks weekly limits of 150-200 connections vs. 50-75 for low SSI (LinkedIn Official).
```

**Source Citations:** [S055] Frank Furness (2026-01-30) — posting frequency benchmarks; [S139] LinkedIn Official SSI Documentation — SSI impact on limits; [S059] Dave Nelson (2026-03-10) — analytics interpretation.


### 7.13 Prompt Point 12: Newsletter Issue Planning

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Newsletter Issue Planning |
| **Input Variables** | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `key_value_proposition`, `existing_audience_size` |
| **Output Format** | Newsletter outline + subject line options + content blocks + CTA |
| **Suggested Strategy** | Chain-of-thought with content block sequencing |
| **Parameters Affected** | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `key_value_proposition` |

**Example Prompt Template:**

```
Plan a LinkedIn newsletter issue for a {target_industry} audience.

CONTEXT:
- Industry: {target_industry}
- Target persona: {target_persona}
- Tone: {brand_voice}
- Content tone: {content_tone}
- Value proposition: {key_value_proposition}
- Current subscribers: {existing_audience_size}

NEWSLETTER REQUIREMENTS:
- Weekly cadence
- Push notifications to all subscribers
- 48-hour distribution window (vs. 24h for regular posts)
- Educational foundation with soft CTA
- Include: hook, main insight, practical framework, case study or example, CTA
- Subject line: curiosity-inducing, under 60 characters

OUTPUT FORMAT:
Return a JSON object with:
- "subject_line_options": array of 3 strings
- "outline": array of objects { "section": string, "content": string, "word_count": integer }
- "cta": string
- "predicted_open_rate": string (benchmark: newsletters get higher engagement than regular posts)

EVIDENCE: LinkedIn newsletters get push notifications to all subscribers and featured placement in search results. B2B companies build subscriber lists of 5,000+ in niche industries, converting 2-3% into qualified leads (Outdoor Advertising, 2026).
```

**Source Citations:** [S067] Outdoor Advertising (2026-01-13) — newsletter performance; [S055] Frank Furness (2026-01-30) — distribution mechanics.




> **Cross-Reference:** For the complete parameterization schema referenced throughout this Prompt Surface Map, see [Section 8: Parameterization Schema](#section-8-parameterization-schema).