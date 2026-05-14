---
section: 7
title: Prompt Surface Map
word_count: 3635
---

## Section 7: Prompt Surface Map

This section documents every pipeline point where an LLM prompt is required to execute the LinkedIn growth strategy. Each prompt point includes input variables, output format, suggested strategy, example template, and the parameters that affect its behavior.

### 7.1 Design Principles

The prompt surface follows three design principles derived from the evidence base:

1. **Context-Rich Inputs:** The 2026 LinkedIn algorithm ("360 Brew") uses LLMs to evaluate content quality and profile authority [S072]. Prompts must therefore supply sufficient context about the user's profile, target audience, and strategic goals to produce outputs that satisfy algorithmic evaluation.

2. **Structured Output Formats:** LinkedIn's distribution system rewards "frameworks, practical checklists, specific guidance — things people bookmark" [S055]. Prompts should enforce structured output formats (JSON, markdown tables, numbered lists) that align with high-performing content patterns.

3. **Chain-of-Thought for Complex Tasks:** Multi-step sequences (outreach campaigns, content calendars) benefit from chain-of-thought reasoning to maintain coherence across outputs [S058]. Simple tasks (headline generation) can use few-shot prompting.

---

### 7.2 Prompt Point 1: Profile Headline Generation

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Profile Headline Generation |
| **Input Variables** | `target_industry`, `target_persona`, `key_value_proposition`, `seniority_level`, `geography`, `language`, `competitor_accounts` |
| **Output Format** | 5 headline variants (120 characters max each) + 1 recommended choice with rationale |
| **Suggested Strategy** | Few-shot with examples from high-SSI profiles in the same industry |
| **Parameters Affected** | `target_industry`, `target_persona`, `seniority_level`, `key_value_proposition`, `brand_voice` |

**Example Prompt Template:**

```
You are a LinkedIn profile optimization expert. Generate 5 headline options for a {seniority_level} {target_persona} in the {target_industry} industry.

CONTEXT:
- Target audience: {target_persona} in {target_industry}
- Value proposition: {key_value_proposition}
- Geography: {geography}
- Tone: {brand_voice}
- Competitor examples: {competitor_accounts}

REQUIREMENTS:
- Maximum 120 characters (mobile-friendly)
- Include role + outcome keywords for searchability
- Front-load the most important keywords
- Use the formula: [Role] | I help [who] achieve [result] through [method]
- Avoid vague language like "I help businesses grow" (algorithm penalizes this)

OUTPUT FORMAT:
Return a JSON object with:
- "headlines": array of 5 strings
- "recommended": index of best option (0-4)
- "rationale": 2-3 sentences explaining the recommendation
- "keywords_included": array of search keywords present

EVIDENCE: The headline is "one of the most heavily weighted fields in LinkedIn search" and should signal both what the user does and what kind of work they seek (Cox, 2026).
```

**Source Citations:** [S060] Sammi Cox / Fonzi AI (2026-04-08) — headline search weight; [S072] Sean Winter (2026-03-27) — 360 Brew algorithm profile reading; [S085] Melanie Goodman (2026-01-21) — cross-section keyword sync.

---

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

---

### 7.4 Prompt Point 3: Connection Request Message Personalization

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Connection Request Message Personalization |
| **Input Variables** | Prospect profile data (name, company, title, recent activity), `target_industry`, `message_personalization_depth`, `language`, `brand_voice` |
| **Output Format** | 3 message variants (under 200 characters each) + personalization signal used |
| **Suggested Strategy** | Few-shot with signal-based personalization levels |
| **Parameters Affected** | `message_personalization_depth`, `target_industry`, `language`, `brand_voice`, `connection_daily_limit` |

**Example Prompt Template:**

```
Generate a personalized LinkedIn connection request note for {prospect_name} at {prospect_company}.

PROSPECT CONTEXT:
- Name: {prospect_name}
- Title: {prospect_title}
- Company: {prospect_company}
- Industry: {prospect_industry}
- Recent activity: {recent_post_topic}
- Mutual connections: {mutual_count}

PERSONALIZATION DEPTH: {message_personalization_depth}
- "basic": Use name + company only
- "contextual": Reference recent post or company news
- "hyper": Combine multiple signals + specific value proposition

REQUIREMENTS:
- Under 200 characters (mobile-friendly, under 250 max)
- No sales pitch in first message
- Include one specific signal (recent post, job change, company news)
- Match tone to {brand_voice}
- Avoid generic compliments like "Impressive background"

OUTPUT FORMAT:
Return a JSON object with:
- "messages": array of 3 strings
- "signal_used": string (what specific detail was referenced)
- "character_counts": array of integers
- "recommended": index of best option

EVIDENCE: Level 3 contextual personalization achieves 30-40%+ acceptance vs. 5-10% for generic messages (Fakhar, 2026). Connection requests with basic personalization outperform generic templates by 3x (Pareto synthesis).
```

**Source Citations:** [S058] Talha Fakhar (2026-03-13) — personalization levels and acceptance rates; [S050] Tai Freligh (2026-02-05) — 80/week safe limit; [S064] Zaidan Ahmad (2025-11-08) — connection request performance.

---

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

---

### 7.6 Prompt Point 5: Content Idea Generation

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Content Idea Generation |
| **Input Variables** | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `key_value_proposition`, `competitor_accounts`, `existing_audience_size`, `geography` |
| **Output Format** | 10 content ideas categorized by format (carousel, text, video, story) + predicted performance |
| **Suggested Strategy** | Few-shot with high-performing examples from the same industry |
| **Parameters Affected** | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `key_value_proposition` |

**Example Prompt Template:**

```
Generate 10 LinkedIn content ideas for a {seniority_level} {target_persona} in {target_industry}.

CONTEXT:
- Target audience: {target_persona} in {target_industry}
- Value proposition: {key_value_proposition}
- Tone: {brand_voice}
- Content tone: {content_tone}
- Current audience size: {existing_audience_size}
- Competitor content examples: {competitor_accounts}

CONTENT REQUIREMENTS:
- Mix: 40% educational, 40% authority/case study, 20% conversion
- Educational content: practical, actionable guidance; industry shifts explained + what to do
- Story content: personal transformation, vulnerable moments, career stories
- Contrarian takes: stand for something, stand against something (with evidence)
- Avoid: external links (reduce reach by 60%), single-image posts (-30% reach), engagement bait
- Preferred formats: PDF carousels (highest performance), short video (<90s), text-only posts

OUTPUT FORMAT:
Return a JSON object with:
- "ideas": array of objects, each with { "title": string, "format": string, "angle": string, "predicted_performance": "high|medium|low", "hook": string }
- "content_pillar_alignment": string (how ideas map to the 3-pillar framework)

EVIDENCE: PDF carousels and short-form video have the highest conversion rates (Freligh, 2026). Educational content and personal transformation stories outperform generic thought leadership (Mansour, 2026).
```

**Source Citations:** [S050] Tai Freligh (2026-02-05) — format performance; [S083] Iris Mansour (2026-03-26) — content types that drive results; [S055] Frank Furness (2026-01-30) — external link and image penalties.

---

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

---

### 7.8 Prompt Point 7: Comment/Engagement Response

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Comment/Engagement Response |
| **Input Variables** | Original post content, comment text, commenter profile, `brand_voice`, `language`, `automation_level` |
| **Output Format** | Reply text (40+ words recommended) + engagement strategy note |
| **Suggested Strategy** | Few-shot with Loop-Back Method examples |
| **Parameters Affected** | `brand_voice`, `language`, `automation_level` |

**Example Prompt Template:**

```
Generate a reply to a LinkedIn comment using the Loop-Back Method.

ORIGINAL POST: {post_content}

COMMENT FROM {commenter_name}: {comment_text}

CONTEXT:
- Your tone: {brand_voice}
- Language: {language}
- Automation level: {automation_level}

LOOP-BACK METHOD REQUIREMENTS:
1. Appreciation: Thank the commenter for their insight
2. Bonus value: Add one additional insight, example, or resource
3. Open question: End with a question that invites further engagement
- Minimum 40 words (quality comments trigger 1.8x more reach)
- Reply within first 60 minutes of posting for algorithmic boost

OUTPUT FORMAT:
Return a JSON object with:
- "reply_text": string
- "appreciation": string
- "bonus_value": string
- "open_question": string
- "word_count": integer
- "strategy_note": string (when to use this reply vs. a simple thank-you)

EVIDENCE: The Loop-Back Method (appreciation + bonus value + open question) unlocks Phase 3 viral distribution (50K-200K impressions). Quality comments of 40+ words trigger 1.8x more reach into the commenter's target niche (HypergrowthAI, 2026).
```

**Source Citations:** [S053] HypergrowthAI (2026-02-03) — Loop-Back Method and comment mechanics; [S050] Tai Freligh (2026-02-05) — quality comment threshold.

---

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

---

### 7.10 Prompt Point 9: DM Conversion Message

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | DM Conversion Message |
| **Input Variables** | Prospect profile, conversation history, `goal_type`, `brand_voice`, `message_personalization_depth`, `risk_tolerance` |
| **Output Format** | Conversion message + alternative softer version + predicted response rate |
| **Suggested Strategy** | Few-shot with proven conversion frameworks |
| **Parameters Affected** | `goal_type`, `brand_voice`, `message_personalization_depth`, `risk_tolerance` |

**Example Prompt Template:**

```
Write a LinkedIn DM conversion message for a prospect who has engaged with 2+ pieces of content.

PROSPECT: {prospect_name}, {prospect_title}, {prospect_company}
CONVERSATION HISTORY: {conversation_history}

CONTEXT:
- Goal type: {goal_type}
- Tone: {brand_voice}
- Personalization depth: {message_personalization_depth}
- Risk tolerance: {risk_tolerance}

CONVERSION FRAMEWORK:
1. Acknowledge the relationship/context
2. Explain who you help (1 sentence)
3. Soft CTA: ask permission to share ideas or offer value

REQUIREMENTS:
- Maximum 3 sentences for first conversion message
- No hard sales pitch
- Include one specific signal from conversation history
- Offer clear value (resource, teardown, consultation)
- Match tone to {brand_voice}

OUTPUT FORMAT:
Return a JSON object with:
- "direct_message": string (clearer CTA)
- "soft_message": string (more indirect)
- "predicted_response_rate": string ("high" 20-30%, "medium" 10-20%, "low" <10%)
- "value_offer": string (what value is being offered)
- "follow_up_timing": string (when to follow up if no response)

EVIDENCE: The first message framework (acknowledge randomness, explain who you help, ask permission) achieves 15-25% reply rates on well-timed messages (Indie Hackers, 2025).
```

**Source Citations:** [S023] Indie Hackers (2025-10-28) — first message framework; [S058] Talha Fakhar (2026-03-13) — reply rate benchmarks; [S067] Outdoor Advertising (2026-01-13) — lead magnet strategy.

---

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

---

### 7.12 Prompt Point 11: Skills & Keyword Optimization

| Attribute | Specification |
|-----------|--------------|
| **Pipeline Point Name** | Skills & Keyword Optimization |
| **Input Variables** | `target_industry`, `target_persona`, `seniority_level`, `key_value_proposition`, `competitor_accounts` |
| **Output Format** | Ordered list of 50 skills + keyword mapping + cross-section sync recommendations |
| **Suggested Strategy** | Zero-shot with explicit constraints |
| **Parameters Affected** | `target_industry`, `target_persona`, `seniority_level`, `key_value_proposition` |

**Example Prompt Template:**

```
Generate an optimized LinkedIn Skills section for a {seniority_level} {target_persona} in {target_industry}.

CONTEXT:
- Industry: {target_industry}
- Target persona: {target_persona}
- Seniority: {seniority_level}
- Value proposition: {key_value_proposition}
- Competitor skills: {competitor_accounts}

REQUIREMENTS:
- Exactly 50 skills (maximum allowed)
- Ordered by relevance (LinkedIn weights top skills more heavily)
- Include a mix of: technical skills, soft skills, industry-specific terms, tools/platforms
- Align with headline and About section keywords for cross-section coherence
- Use the prompt: "If I were hiring for this role, what keywords would I search?"

OUTPUT FORMAT:
Return a JSON object with:
- "skills": array of 50 strings (ordered by priority)
- "top_10": array of strings (highest priority)
- "keyword_mapping": object (which skills map to headline/About keywords)
- "cross_section_sync": string (recommendations for aligning with other profile sections)

EVIDENCE: LinkedIn uses listed skills to determine which searches a profile appears in, and the order of skills matters for weighting (Cox, 2026). Cross-sectional keyword coherence is critical for the 2026 algorithm (Winter, 2026).
```

**Source Citations:** [S060] Sammi Cox / Fonzi AI (2026-04-08) — skills ordering and search weight; [S072] Sean Winter (2026-03-27) — cross-section authority checking.

---

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

---

### 7.14 Prompt Point Summary Table

| # | Pipeline Point | Strategy | Parameters Affected | Key Evidence Source |
|---|---------------|----------|---------------------|---------------------|
| 1 | Profile Headline Generation | Few-shot | `target_industry`, `target_persona`, `seniority_level`, `key_value_proposition`, `brand_voice` | [S060], [S072] |
| 2 | Profile About Section Generation | Chain-of-thought | `target_industry`, `target_persona`, `key_value_proposition`, `brand_voice` | [S079], [S072] |
| 3 | Connection Request Personalization | Few-shot | `message_personalization_depth`, `target_industry`, `language`, `brand_voice` | [S058], [S064] |
| 4 | Follow-Up Sequence Generation | Chain-of-thought | `goal_type`, `risk_tolerance`, `automation_level`, `brand_voice` | [S065], [S058] |
| 5 | Content Idea Generation | Few-shot | `target_industry`, `target_persona`, `brand_voice`, `content_tone` | [S050], [S083] |
| 6 | Content Draft Writing | Chain-of-thought | `target_industry`, `target_persona`, `brand_voice`, `content_tone`, `language` | [S072], [S055] |
| 7 | Comment/Engagement Response | Few-shot | `brand_voice`, `language`, `automation_level` | [S053], [S050] |
| 8 | Lead Qualification Scoring | Structured output | `target_industry`, `target_persona`, `seniority_level`, `goal_type` | [S050], [S064] |
| 9 | DM Conversion Message | Few-shot | `goal_type`, `brand_voice`, `message_personalization_depth`, `risk_tolerance` | [S023], [S058] |
| 10 | Weekly Growth Report Summary | Structured output | `goal_type`, `time_to_goal_weeks`, `phase`, `content_capacity` | [S055], [S139] |
| 11 | Skills & Keyword Optimization | Zero-shot | `target_industry`, `target_persona`, `seniority_level`, `key_value_proposition` | [S060], [S072] |
| 12 | Newsletter Issue Planning | Chain-of-thought | `target_industry`, `target_persona`, `brand_voice`, `content_tone` | [S067], [S055] |

---



> **Cross-Reference:** For the complete parameterization schema referenced throughout this Prompt Surface Map, see [Section 8: Parameterization Schema](#section-8-parameterization-schema).

