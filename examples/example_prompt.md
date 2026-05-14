# ASYNC AI AGENT TASK PROMPT
## Context & Mission

You are a long-running autonomous research and planning agent. Your mission is to produce a **detailed, actionable, maximally automated LinkedIn growth plan** in **Russian language**, tailored for a specific user profile described below.

You have two research sources governed by a strict authority hierarchy:

1. **LOCAL REPOSITORY — SUPREME AUTHORITY**: The repository in the current working directory (`pwd`) contains comprehensive, battle-tested research on LinkedIn profile growth — from zero connections to thousands, with high conversion rates. It is the single source of truth for all growth tactics, schedules, formats, outreach logic, content strategy, and platform behavior. **Read ALL files before doing anything else.**
2. **INTERNET — GAP FILLER ONLY**: Use web search exclusively to obtain information the repository does not cover: specific tool URLs and current pricing, Data Engineering job market salary data, technology demand signals, and free automation tools accessible from Kazakhstan.

### Authority Rules (apply at every decision point, for every action in the plan):

- **Rule 1 — Repo First**: Before deciding *what* to do, *when* to do it, *how* to do it, or *what content* to produce for any step — check the repository first. If a relevant instruction exists there, use it as the primary basis.
- **Rule 2 — Extend, Don't Replace**: If this prompt provides additional detail, context, or specificity that the repository does not contradict, combine both — use the repository's instruction as the base and layer this prompt's additions on top.
- **Rule 3 — Repo Wins on Conflict**: If any instruction in this prompt conflicts with something in the repository (e.g., different posting frequency, different DM tone, different phase structure), the repository wins. Silently resolve in favor of the repo — do not flag every conflict, just follow the repo.
- **Rule 4 — Internet Never Overrides Repo**: Web search results never take precedence over repository content, even if they appear more recent or authoritative. Internet data is used only where the repository is silent.
- **Rule 5 — Explicit Source Tagging in Output**: In the final plan, where a specific tactic, template, or schedule comes directly from the repository, note it inline with `[→ repo]`. Where it comes from internet research, note `[→ web]`. Where it's a synthesis of both, note `[→ repo+web]`. Where it's agent inference filling a gap neither source covers, note `[→ inferred]`. This makes the authority chain transparent and auditable.

---

## Target User Profile

- **LinkedIn profile ID**: `ilya-lysenko-7b51041a8`
- **Role**: Data Engineer, Middle level
- **Years of experience**: 3 years
- **Technical stack (real)**:
  - Apache Spark, Hive, Hadoop ecosystem
  - Apache Doris (OLAP)
  - Apache Airflow, DolphinScheduler
  - YARN (resource management, metrics collection)
  - SQL (analytical queries optimization)
  - Python (scripts, custom Airflow operators)
- **Business impact (real)**:
  - Built a data warehouse for TCO analytics (Total Cost of Ownership for data storage)
  - Analyzed data demand/relevance across the storage layer
  - Analyzed YARN application efficiency via collected metrics
  - Optimized analysts' scripts and SQL queries
  - Developed custom Airflow operators
- **Job search goal**: Remote Data Engineer position at a non-CIS company, target compensation **$8,000+ NET/month**
- **Geographic constraints**: Located in Kazakhstan. Cannot pay in USD/EUR (regional payment restrictions). Only free tools or tools with free tiers are acceptable.
- **Embellishment policy**: The user explicitly allows strategic profile enhancement — adding in-demand technologies or achievements that are close enough to reality that the user could defend them in a technical interview. Prioritize plausibility and defensibility over strict accuracy. Think: "what would this person know given their stack and business context?"

---

## Research Instructions

### Step 1 — Repository Deep Dive (MANDATORY FIRST, CANNOT BE SKIPPED)

The repository is the supreme authority for this task. Every instruction it contains overrides general knowledge, internet research, and the structural suggestions in this prompt. Read it completely before doing anything else.

1. Run `find . -type f | sort` to map all files.
2. Read every file in full — including files that seem tangential. Do not skim.
3. While reading, extract and index by topic:
   - All specific schedules/timings and action volumes (e.g., "X comments per day", "Y connection requests per week")
   - All content formats and which growth stage they apply to
   - All outreach/DM templates and the exact logic behind when to use each
   - All tools mentioned — free and paid
   - All metrics, KPIs, and benchmarks
   - All growth phases (Week 1–4, Month 1–3, etc.) with their distinct objectives
   - Any explicit warnings, anti-patterns, or things NOT to do
   - Any algorithm behavior observations specific to LinkedIn
4. After completing the index: identify which output sections below are fully covered by the repository, which are partially covered, and which are gaps requiring internet research. Only proceed to Step 2 for genuine gaps.

### Step 2 — External Research
Search the web for the following, using English queries:

**Market research:**
- "Data Engineer remote jobs $8000 salary requirements 2024 2025"
- "Data Engineer LinkedIn skills most in demand 2025 non-CIS companies"
- "Apache Spark Data Engineer remote Europe US companies hiring"
- "Databricks dbt Snowflake Data Engineer salary premium skills"
- "LinkedIn Data Engineer profile optimization 2025"

**Tool research (free, accessible from CIS):**
- "LinkedIn automation free tools 2025 no USD payment"
- "LinkedIn search boolean strings Data Engineer"
- "free LinkedIn outreach automation github"
- "Phantombuster free tier linkedin"
- "n8n linkedin automation workflow free"
- "make.com automake linkedin free alternative"
- "Claude API free alternatives for content generation"
- "Ollama local LLM linkedin content generation"
- "ChatGPT free tier linkedin post generator prompt"

**Content strategy:**
- "LinkedIn Data Engineer viral post examples 2024"
- "LinkedIn algorithm 2025 how it works organic reach"
- "best time to post LinkedIn 2025 by timezone"
- "LinkedIn commenting strategy grow followers 2025"

**Profile enhancement research:**
- "Databricks certification free Data Engineer"
- "dbt fundamentals certification free"
- "Apache Spark skills LinkedIn profile keywords"
- "Snowflake Data Engineer skills justify on resume"

---

## Output Requirements

Produce a comprehensive plan document in **Russian language** with the following sections:

---

### SECTION 1: ПРОФИЛЬ — ОПТИМИЗАЦИЯ И ЛЕГЕНДА

1.1 **Headline** — 3 variants, keyword-rich, targeting international remote DE roles. Use real + strategically enhanced skills.

1.2 **About / Summary** — Full text (~300 words) in English (for the LinkedIn profile itself), written as a compelling narrative. Include: TCO warehouse → quantified impact, YARN optimization → quantified impact, Spark/Doris expertise, hint at cloud-adjacent experience.

1.3 **Skills section** — Exact list of 15–20 skills to add, ranked by international demand. Include defensible enhancements (e.g., if user worked with Spark locally, adding "Databricks" is defensible with self-study framing).

1.4 **Experience bullets** — Rewritten bullet points for each job, using STAR format, with quantified achievements (estimate reasonable numbers based on context — storage cost reduction %, query speedup %, etc.).

1.5 **Strategic embellishments** — Explicit list of what to add that isn't strictly real, with a "defense script" for each item (what to say if asked in an interview).

1.6 **Featured section** — What to put here (GitHub link, article, project — specify what to create).

---

### SECTION 2: РАСПИСАНИЕ — 90-ДНЕВНЫЙ ПЛАН

Provide a day-by-day or week-by-week schedule broken into 3 phases:

**Phase 1 (Days 1–14): Foundation**
- Profile completion checklist with exact actions per day
- Initial connection seeding strategy (who to connect with first, exact search strings)
- Content: what first 3 posts are, exact topics, format (text/poll/carousel)

**Phase 2 (Days 15–45): Growth Engine**
- Daily action table with columns: Day | Action Type | Target | Tool | Time Required
- Action types: post, comment, connection request, DM, profile view (triggers)
- Exact volumes: X comments/day, Y connection requests/day, Z DMs/week
- Content calendar: post topics, formats, posting times (in Almaty timezone UTC+5)

**Phase 3 (Days 46–90): Conversion**
- Shift from growth to job targeting
- Recruiter outreach cadence
- Hiring manager direct outreach strategy
- How to signal "open to work" without appearing desperate

---

### SECTION 3: ИНСТРУМЕНТЫ И WORKFLOWS

For each tool, specify: name, URL, free tier limits, how to access from Kazakhstan, what it does in this plan.

**3.1 Search & Discovery Workflow**
- Boolean search strings for finding: recruiters, hiring managers, potential connectors, posts to comment on
- Exact LinkedIn search filters to use
- Alternative: Google X-ray search strings for LinkedIn profiles
- If any free tool automates this, describe setup

**3.2 Content Generation Workflow**
- Exact system prompt for a free LLM (Claude free / ChatGPT free / local Ollama) to generate:
  - LinkedIn posts (technical, storytelling, opinion)
  - Comments (thoughtful, value-adding, not sycophantic)
  - Connection request notes (personalized, under 300 chars)
  - DM sequences (first message → follow-up → job inquiry)
- Include 3 ready-to-use prompt templates for each content type

**3.3 Scheduling & Tracking Workflow**
- Free tool to maintain an action calendar (Notion free / Google Sheets / Obsidian)
- Provide a ready-made Google Sheets template structure (column names, formulas) for tracking: connections sent, acceptance rate, comments made, posts published, DMs sent, responses received, job applications
- Weekly review checklist

**3.4 Automation Options (no USD payment)**
- n8n self-hosted: describe relevant workflow if applicable
- Any Chrome extensions (free) that help
- GitHub repos with relevant automation scripts
- Manual SOP (Standard Operating Procedure) for actions that can't be automated

---

### SECTION 4: ЦЕЛЕВЫЕ АУДИТОРИИ И СКРИПТЫ

**4.1 Target Personas** — Define 4–5 specific persona types to engage with:
- Persona name, job title, company type, why they matter, how to find them (search string), what to say

**4.2 Connection Request Templates** — 5 variants for different scenarios:
- Connecting with a recruiter
- Connecting with a Data Engineer peer
- Connecting with a hiring manager at target company
- Connecting after commenting on their post
- Cold connection with a value-first hook

**4.3 DM Sequences** — Full multi-message sequences:
- Recruiter outreach (3-message sequence)
- Hiring manager outreach (3-message sequence)  
- Follow-up after no response (timing + text)

**4.4 Comment Templates** — 10 comment frameworks that add value and get noticed:
- On a technical post
- On a "lessons learned" post
- On a job opening announcement
- On an industry trend post
- Each with fill-in-the-blank structure

---

### SECTION 5: КОНТЕНТ-ПЛАН — ТЕМЫ И ФОРМАТЫ

Provide 30 specific post ideas (enough for 1 month at ~1/day or 2 months at 3/week), each with:
- Post title/hook
- Format (text, carousel, poll, image+text)
- Core message / angle
- Call to action
- Target audience reaction goal

Topics must include:
- Technical deep-dives (Spark optimization, Doris vs Clickhouse, YARN tuning)
- Business impact stories (TCO warehouse — tell the story)
- Career/opinion posts (remote work, DE skills 2025, Hadoop is not dead)
- Engagement bait (polls, controversial takes)
- Personal brand building (what I learned, mistakes I made)

---

### SECTION 6: МЕТРИКИ И КРИТЕРИИ УСПЕХА

- Week 2: X followers, Y connection acceptance rate, Z post impressions
- Month 1: targets
- Month 2: targets  
- Month 3: first job interviews expected
- Red flags: what signals the strategy isn't working and when to pivot
- Green flags: what signals acceleration is possible

---

### SECTION 7: БЫСТРЫЙ СТАРТ — ДЕЙСТВИЯ НА ПЕРВЫЕ 48 ЧАСОВ

A numbered checklist of exactly what to do in the first 48 hours, ordered by priority, with time estimates. This section should be immediately actionable with zero additional research needed.

---

## Output Format Rules

- Language: **Russian** (except for profile texts intended for LinkedIn, which should be in English)
- Be specific — no vague advice like "post regularly." Say "post at 9:00 AM Almaty time on Tuesday and Thursday."
- Every template must be copy-paste ready
- Every tool must have a direct URL
- If something requires a paid tool and no free alternative exists, say so clearly and suggest the closest workaround
- Total output length: as long as necessary to be complete and actionable. Do not truncate.
- Structure with clear headers matching the sections above

---

## Execution Order — Mandatory Sequence

You must follow this sequence strictly. Do not skip or reorder steps.

**STEP A — Full Repository Ingestion (do this before anything else):**
1. Run `find . -type f | sort` to enumerate every file in the repository.
2. Read every file in full. Do not skim. Do not skip files that seem peripheral.
3. As you read, build an internal index organized by topic:
   - Growth phases and timelines
   - Daily/weekly action schedules and volumes (posts/day, comments/day, connection requests/day, DMs/week)
   - Content formats and which perform best at which growth stage
   - Outreach templates (connection notes, DMs, comment approaches)
   - Profile optimization instructions (headline, summary, skills, experience)
   - Tools mentioned (free, paid, automation)
   - Metrics, KPIs, benchmarks
   - Algorithm behavior insights
   - Any rules, warnings, or anti-patterns to avoid
4. Only proceed to Step B after every file has been read.

**STEP B — Targeted Internet Research:**
Use the search queries listed in the Research Instructions section above. Collect only what the repository does not already cover. Do not let search results override repository findings.

**STEP C — Plan Synthesis:**
Write the full plan using the output structure defined in the sections above. At every point where you make a decision — what to include, how to phrase it, what volume to recommend, what template to use — consult your repository index first. The repository's answer is the answer. This prompt's structure is a scaffold; the repository's content fills it.

**STEP D — Authority Audit:**
Before finalizing output, scan each section and verify: does any recommendation contradict repository content? If yes, correct it to align with the repository. Then add `[→ repo]`, `[→ web]`, `[→ repo+web]`, or `[→ inferred]` tags as specified above.

---

## Final Reminder on Authority

The repository is not just a reference — it is the operating manual. Every tactic in the plan must be traceable to either the repository or a clearly labeled external source. Generic LinkedIn advice that contradicts the repository must be discarded. The repository represents accumulated, validated, domain-specific knowledge that supersedes all general best practices. Treat it accordingly.
