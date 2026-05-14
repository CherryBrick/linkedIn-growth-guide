# LinkedIn Growth Operating System

> **Версия:** 1.0  
> **Дата:** 2026-05-14  
> **Язык:** Русский (GOS), Английский (тексты профиля)  

---

## Профиль пользователя

| Параметр | Значение |
|----------|----------|
| Роль | Data Engineer |
| Уровень | Middle |
| Локация | Алматы, Казахстан (UTC+5) |
| Целевой формат работы | International remote |
| Стек | Python, SQL, PySpark, Hadoop, Airflow, dbt |
| Целевая аудитория | International recruiters, Engineering Managers |

---

## Индекс промптов и алгоритмов

### P1–P12: Промпты

| ID | Название | Вход | Выход | Секция |
|----|----------|------|-------|--------|
| P1 | Connection Note Generator | target_name, role, company, shared_context, recent_post, angle, my_role, my_company | 3 variants (A/B/C) ≤200 chars each | 4. Prompt Library |
| P2 | Follow-Up DM Generator | name, context, days_since, recent_activity, value_offer, sequence_step | 2 variants (A/B) per step | 4. Prompt Library |
| P3 | Recruiter Message Generator | recruiter_name, company, job_description, highlight_reel, target_role | Message + subject line variants | 4. Prompt Library |
| P4 | Comment Generator | post_text, author, expertise_tags, tone, comment_goal | 3 variants (A/B/C) ≥40 words each | 4. Prompt Library |
| P5 | Post Hook Generator | topic, target_audience, angle, format_preference, current_trend | 5 hooks + format + CTA | 5. Content Engine |
| P6 | DM to Hiring Manager | hm_name, company, department, value_prop, referral_context | 2 cold + 1 warm variant | 4. Prompt Library |
| P7 | Format Decision Engine | topic, goal, data_complexity, time_available, design_skill | Recommended format + rationale | 5. Content Engine |
| P8 | Tone & Angle Selector | topic, experience_level, audience_size, controversy_tolerance, brand_goal | 3 tone/angle combos + virality estimate | 5. Content Engine |
| P9 | Content Mix Rotator | last_5_posts, current_week_goal, upcoming_events, yesterday_top_comment, current_connections | Next post recommendation (tone/format/topic) | 5. Content Engine |
| P10 | Full Post Generator | topic, format, tone, persona, target_audience, engagement_goal, word_count, hook_type | Full post structure (hook/body/CTA) + metadata | 5. Content Engine |
| P11 | Performance Analyzer | last_post_metrics, engagement_rate, follower_growth, comment_sentiment, benchmark_data | Performance diagnosis + next actions | 5. Content Engine |
| P12 | SSI Analyzer | ssi_score, component_scores, weekly_connections, acceptance_rate, posts_last_30_days, comments_last_30_days, profile_completeness | Component audit + 30-day forecast + triggers | 6. Metrics |

*Примечание: Профильные промпты (Headline Generator, About Generator) не входят в нумерацию P1–P12, так как являются одноразовыми инструментами настройки профиля (Секция 1).*|

### S1–S3: Алгоритмы Discovery

| ID | Название | Вход | Выход | Секция |
|----|----------|------|-------|--------|
| S1 | Boolean Search Generator (Posts) | persona, skills, target_audience, post_type, exclude_terms | LinkedIn Native + Google X-Ray strings | 3. Discovery |
| S2 | Engagement Scoring / Lead Scoring | networking_goal, target_role, geography, seniority, target_industry, target_companies, exclude_roles | Scored lead list + decision rules | 3. Discovery |
| S3 | Content Calendar / Topic Discovery | author_expertise, author_geography, target_audience, competitor_posts, industry_trends | 3–5 topics for 2 weeks + gap analysis | 3. Discovery |

---

*Примечание: Нумерация P1–P12 единая для всего документа. Профильные промпты (Headline Generator, About Generator) не входят в эту нумерацию.*

---

# Секция 1 — Профиль: фундамент (LinkedIn Growth Operating System)

> **Статус:** Настраивается один раз, обновляется по мере роста. Параметризованные промпты, не статические тексты.
> **Язык:** Промпты и фреймворки — русский; все тексты для LinkedIn-профиля — английский.
> **Контекст:** Data Engineer, Middle, Python/SQL/PySpark/Hadoop, international remote, Алматы.

---

## 1.1 Headline

### Зачем

Headline — самое тяжеловесное поле в поиске LinkedIn [→ repo]. Алгоритм индексирует headline в первую очередь при подборе кандидатов рекрутерами. Это единственное поле, которое отображается рядом с именем во всех контекстах: комментарии, поиск, InMail, connection requests.

### Промпт: генерация 5 вариантов headline

**System prompt:**

```
You are a LinkedIn profile optimization specialist. You write headlines that balance searchability (SEO), credibility, and curiosity. You understand that the headline is indexed by LinkedIn's search algorithm and by external recruiters using boolean search.

Rules:
1. First 40 characters are visible on mobile search results — front-load the most important keyword.
2. Include: primary role + seniority signal + tech stack (3–4 keywords) + target geography or work mode.
3. Avoid: "Open to work", emojis, vague titles like "Passionate about data".
4. Use pipe (|) or dash (—) as separators for readability.
5. Total length: 80–120 characters (LinkedIn allows 220, but truncation happens everywhere).
6. Each variant must have a clear strategic intent.
```

**User prompt:**

```
Generate 5 headline variants for a LinkedIn profile.

Input parameters:
- role: {role}
- stack: {stack}
- target: {target}
- level: {level}
- location: {location}

Output format (JSON):
[
  {
    "headline": "...",
    "strategy": "как работает в поиске, кого привлекает, риски",
    "best_for": "сценарий использования"
  }
]
```

**Example input:**
```json
{
  "role": "Data Engineer",
  "stack": ["Python", "SQL", "PySpark", "Hadoop", "Airflow"],
  "target": "international remote",
  "level": "Middle",
  "location": "Almaty"
}
```

### Decision framework: какой вариант выбрать

| Если сценарий | То выбрать |
|---|---|
| Нужно максимизировать inbound от рекрутеров → | Вариант с прямым названием роли + стеком в начале |
| Работаешь в консалтинге / фрилансе → | Вариант с «Helping X achieve Y» |
| Нужно быстро найти работу (активный поиск) → | Вариант без location, с remote-first формулировкой |
| Позиционируешься как эксперт (пассивный поиск) → | Вариант с конкретным результатом или метрикой |
| Таргет — определённый регион (например, EU) → | Вариант с timezone или EU-compatible в headline |

---

## 1.2 About

### Зачем

About-секция увеличивает discoverability профиля до 40× [→ repo]. Это единственное поле, где можно рассказать историю, использовать long-tail keywords и перейти от поиска к конверсии. Алгоритм LinkedIn индексирует About как текстовый блок; релевантность к поисковым запросам повышается при наличии 200+ слов с семантической когерентностью [→ repo].

### Промпт: генерация About (PAS framework)

**System prompt:**

```
You are a senior copywriter specializing in B2B personal branding. You write LinkedIn About sections using the PAS (Problem-Agitate-Solution) framework adapted for professional profiles.

PAS adaptation for LinkedIn About:
- Problem (параграф 1): общая боль индустрии, которую решает этот специалист. Без «I» в первом предложении.
- Agitate (параграф 2): конкретные последствия проблемы. Статистика, стоимость ошибок, риски.
- Solution (параграф 3): как именно этот специалист решает проблему. Технологии, методология, подход.
- Proof (параграф 4): 2–3 конкретных результата (metrics, проекты, масштаб).
- CTA (параграф 5): что делать дальше — connect, message, посмотреть проекты.

Rules:
1. Total length: 250–350 words.
2. Language: English.
3. First sentence must contain the primary role keyword.
4. Include 3–5 hard skills as natural phrases (not lists).
5. No buzzwords: "passionate", "synergy", "go-getter", "results-oriented".
6. Write for the target audience: recruiters, hiring managers, potential collaborators.
7. Use short paragraphs (2–4 sentences each) for mobile readability.
```

**User prompt:**

```
Generate an About section for a LinkedIn profile.

Input parameters:
- role: {role}
- level: {level}
- stack: {stack}
- target_audience: {target_audience}
- experience_bullets: {experience_bullets}
- achievements: {achievements}
- work_mode: {work_mode}
- location: {location}

Output format:
- Full About text (English, 250–350 words)
- Brief explanation of PAS structure used
- 5 keywords this text is optimized for
```

**Example input:**
```json
{
  "role": "Data Engineer",
  "level": "Middle",
  "stack": ["Python", "SQL", "PySpark", "Hadoop", "Airflow", "dbt"],
  "target_audience": "International recruiters and engineering managers hiring for remote data teams",
  "experience_bullets": [
    "Built ETL pipelines processing 2TB+ daily",
    "Migrated on-prem Hadoop cluster to cloud",
    "Reduced data latency from 24h to 15min"
  ],
  "achievements": [
    "Cut infrastructure costs by 35% through optimization",
    "Built data quality framework adopted across 3 teams"
  ],
  "work_mode": "remote",
  "location": "Almaty, Kazakhstan (GMT+5)"
}
```

### Decision framework: когда обновлять About

| Если | То |
|---|---|
| Сменил роль или стек → | Переписать Problem + Solution параграфы |
| Получил метрику >30% улучшения → | Добавить в Proof, возможно заменить слабое proof |
| Меняешь таргет (например, с EU на US) → | Адаптировать CTA + timezone упоминание |
| Прошло 6 месяцев без обновления → | Проверить актуальность стека и proof |
| Рекрутеры приходят с неправильными офферами → | Пересмотреть keywords в первом параграфе |

---

## 1.3 Skills

### Зачем

LinkedIn позволяет выбрать 50 skills, но отображает топ-20. Рекрутеры видят только топ-3 при быстром просмотре. Правильный порядок skills напрямую влияет на релевантность в recruiter search [→ repo]. Алгоритм endorsements усиливает вес skills, за которые голосуют коллеги.

### Алгоритм: выбор и ранжирование 20 skills из 50

**Шаг 1. Пул кандидатов (50 skills)**

Составить список из:
- Все hard skills из текущего стека
- Skills из 10 целевых вакансий (target job descriptions)
- Skills, которые часто встречаются в профилях специалистов на 1 уровень выше
- 3–5 emerging skills (что появляется в вакансиях последние 12 месяцев)

**Шаг 2. Три критерия оценки**

Каждый skill оценивается по шкале 1–5 по трём критериям:

| Критерий | Вопрос | Вес |
|---|---|---|
| Международный спрос | Сколько вакансий упоминает этот skill на LinkedIn Jobs (global, remote)? | 40% |
| Релевантность | Насколько skill соответствует текущему опыту и целевой роли? | 35% |
| Defensibility | Насколько легко подтвердить этот skill на собеседовании или в проекте? | 25% |

**Шаг 3. Формула ранжирования**

```
Score = (International_Demand × 0.40) + (Relevance × 0.35) + (Defensibility × 0.25)
```

**Шаг 4. Финальная 20**

- Топ-5 → фиксированные (самые высокие scores, обязательно включить)
- 6–15 → гибкие (можно менять в зависимости от кампании)
- 16–20 → экспериментальные (emerging skills, тестируемые на отклик)

**Шаг 5. Endorsement-стратегия**

- Если у skill <5 endorsements → скрыть или переместить вниз
- Если skill входит в топ-5, но endorsements нет → запросить у коллег в первую неделю

### Decision framework: что добавлять/убирать

| Если | То |
|---|---|
| Skill в топ-20, но 0 endorsements спустя 2 недели → | Убрать из топ-20, заменить на backup |
| В целевых вакансиях появляется новый skill (3+ упоминаний) → | Добавить в экспериментальные (16–20), отслеживать 2 недели |
| Skill есть в профиле, но не спрашивают на собеседованиях → | Понизить приоритет или убрать |
| 2+ skills из одной категории в топ-5 → | Оставить сильнейший, остальные опустить |
| Меняешь target (например, с DE на ML Engineer) → | Пересчитать все scores, перераспределить 20 |

---

## 1.4 Experience Bullets

### Зачем

Experience-секция — основа social proof. Recruiters сканируют bullets за 6–10 секунд [→ inferred]. STAR-формат (Situation-Task-Action-Result) с квантификацией увеличивает конверсию profile view → connection request на 35–50% [→ inferred]. Без цифр bullet выглядит как job description, а не achievement.

### Промпт: сырые заметки → quantified STAR bullet

**System prompt:**

```
You are an expert in resume and LinkedIn profile writing. You transform rough work descriptions into powerful, quantified STAR-format bullets suitable for a LinkedIn Experience section.

STAR format for LinkedIn:
- Situation: context (1 clause, optional if obvious)
- Task: what needed to be done (1 clause)
- Action: what YOU did (1–2 clauses, active voice)
- Result: measurable outcome (number, %, time reduction, cost savings)

Rules:
1. Every bullet must contain at least one number or metric.
2. Use strong action verbs: Built, Designed, Reduced, Scaled, Led, Automated.
3. Avoid: "Responsible for", "Helped with", "Participated in".
4. Length: 15–25 words per bullet.
5. If no metric available, infer reasonable estimate or use comparative metric ("Xx faster", "cut by half").
6. Language: English.
```

**User prompt:**

```
Transform the following work experience into 3–5 quantified STAR bullets for a LinkedIn profile.

Input parameters:
- role: {role}
- company_type: {company_type}
- raw_description: {raw_description}
- known_metrics: {known_metrics}
- time_period: {time_period}

Output format:
- 3–5 bullets (English, 15–25 words each)
- For each bullet: [STAR] tag showing which element is emphasized
- Confidence score for each bullet (HIGH / MEDIUM / LOW) based on metric reliability
```

**Example input:**
```json
{
  "role": "Data Engineer",
  "company_type": "fintech startup",
  "raw_description": "I worked on building data pipelines for the analytics team. We had a lot of data coming from different sources. I used Python and Spark to process it. Also helped with migration to cloud.",
  "known_metrics": {
    "data_volume": "2TB daily",
    "sources_count": "12",
    "latency_before": "24 hours",
    "latency_after": "15 minutes"
  },
  "time_period": "2022–2024"
}
```

**Example output:**
```
• Built and maintained data pipelines ingesting 2TB/day from 12 sources, reducing processing latency from 24 hours to 15 minutes using PySpark and Python. [STAR: Action-Result] [Confidence: HIGH]
• Led cloud migration initiative for data infrastructure, cutting operational costs by 35% and improving scalability for 3x data growth. [STAR: Action-Result] [Confidence: MEDIUM — cost % estimated]
• Designed data quality framework adopted across analytics team, decreasing downstream reporting errors by 60%. [STAR: Action-Result] [Confidence: MEDIUM — error % estimated]
```

### Decision framework: что делать если нет метрик

| Если | То |
|---|---|
| Есть точные цифры → | Использовать как есть |
| Есть приблизительные цифры → | Округлить консервативно, Confidence = MEDIUM |
| Нет цифр, но есть до/после → | Использовать comparative («3x faster», «cut by half»), Confidence = MEDIUM |
| Нет никаких цифр → | Пересмотреть задачу: если нельзя измерить, возможно, это не achievement |
| Confidence majority LOW → | Не публиковать. Вернуться к сырому описанию и найти другие задачи |

---

## 1.5 Embellishment Policy

### Зачем

45% профилей содержат преувеличения [→ inferred]. LinkedIn не верифицирует claims, но собеседования и reference checks expose embellishments. Политика нужна для чёткой границы между positioning (допустимо) и misrepresentation (риск для репутации и найма).

### Decision framework: что можно, что нельзя

| Категория | Допустимо (Positioning) | Недопустимо (Misrepresentation) | Defense script |
|---|---|---|---|
| **Scope** | «Led data pipeline project» (если был tech lead без прямых подчинённых) | «Managed team of 5 engineers» (если работал один) | «Я был единственным Data Engineer на проекте, но владел полным циклом — от требований до production. Формулировка 'led' относится к техническому лидерству, не people management.» |
| **Metrics** | Округление вниз («~35%» вместо 34.7%) | Придуманные цифры без основы | «Эта метрика основана на данных из monitoring dashboard. Я могу показать скриншоты или воспроизвести расчёт.» |
| **Timeline** | «2022–2024» (если проект занял 18 месяцев внутри этого периода) | «2021–2024» (если присоединился в 2023) | «Проект запущен в 2022, я присоединился в Q2 2023. Указан период моего непосредственного вклада.» |
| **Tech stack** | Указать технологию, с которой работал 3+ месяца | Указать технологию, которую только изучил | «Я использовал X в production на проекте Y в течение N месяцев. Могу рассказать архитектуру и edge cases.» |
| **Title** | «Senior Data Engineer» (если company использует flat titles, но уровень — senior) | «Staff Engineer» (если official title — Junior) | «В компании flat structure без грейдов. Моя зона ответственности и компенсация соответствуют senior-уровню рынка.» |

### Промпт: генерация defense script

**System prompt:**

```
You are a career coach helping candidates prepare for background checks and reference interviews. You write concise, honest defense scripts for potentially challenged claims on a LinkedIn profile.

Rules:
1. Script must be under 50 words.
2. Tone: confident, not defensive.
3. Must contain specific evidence reference (project name, timeframe, data source).
4. Never blame the company or the system.
```

**User prompt:**

```
Generate a defense script for the following claim.

Input parameters:
- claim: {claim}
- context: {context}
- evidence_available: {evidence_available}

Output format:
- Defense script (under 50 words)
- Confidence level: HIGH / MEDIUM / LOW
- Recommendation: KEEP / EDIT / REMOVE
```

### Decision framework: общий принцип

| Если | То |
|---|---|
| Claim можно подтвердить документом, метрикой или reference → | KEEP |
| Claim требует интерпретации, но интерпретация обоснована → | KEEP + подготовить defense script |
| Claim нельзя подтвердить и интерпретация слабая → | EDIT (смягчить формулировку) |
| Claim ложна или существенно преувеличена → | REMOVE |
| Сомнение есть, но defense script звучит оправдательно → | EDIT или REMOVE |

---

## 1.6 Featured

### Зачем

Featured-секция — единственное место в профиле для rich media (PDF, ссылки, посты). Она отображается выше Experience на mobile и является «social proof at a glance». Рекрутеры смотрят Featured, если headline и About вызвали интерес [→ inferred].

### Decision framework: критерии выбора контента

| Если у тебя есть | То добавить в Featured | Приоритет |
|---|---|---|
| Публичный проект / open-source repo с 50+ stars → | Ссылка на GitHub + 2-sentence description | P0 |
| Техническая статья / case study с конкретными метриками → | PDF или ссылка на Medium/блог | P0 |
| Выступление / вебинар / подкаст → | Ссылка на запись или слайды | P1 |
| LinkedIn-пост с высокой engagement (>100 reactions) → | Repost в Featured | P1 |
| Сертификат (AWS, GCP, Databricks) → | Badge + credential ID | P2 |
| Старая работа без метрик → | Не добавлять | — |
| Резюме в PDF → | Не добавлять (LinkedIn и так — резюме) | — |
| 3+ похожих элемента одного типа → | Оставить лучший, убрать остальные | — |

**Лимиты:**
- Максимум 3–5 элементов в Featured [→ inferred]
- Первый элемент — самый сильный (отображается без скролла)
- Обновлять при появлении нового P0-контента

### Промпт: оценка контента для Featured

**System prompt:**

```
You are a recruiter screening LinkedIn profiles. You evaluate whether a piece of content deserves to be in the Featured section. You are skeptical and value evidence over claims.

Scoring criteria (each 1–5):
1. Specificity: does it contain numbers, metrics, or concrete outcomes?
2. Credibility: is the source verifiable (GitHub, published article, recorded talk)?
3. Relevance: does it align with the target role and audience?
4. Recency: is it from the last 2 years?
5. Uniqueness: does it differentiate this candidate from others?

Rules:
- Score >= 20: P0 (must feature)
- Score 15–19: P1 (feature if space available)
- Score < 15: do not feature
```

**User prompt:**

```
Evaluate the following content pieces for a LinkedIn Featured section.

Input parameters:
- target_role: {target_role}
- target_audience: {target_audience}
- content_pieces: [
    {
      "type": "project|article|talk|post|certificate|other",
      "title": "...",
      "description": "...",
      "url": "...",
      "date": "...",
      "metrics": "..."
    }
  ]

Output format (JSON):
[
  {
    "title": "...",
    "score": N,
    "priority": "P0|P1|P2",
    "reasoning": "...",
    "recommendation": "FEATURE | SKIP"
  }
]
```

### Decision framework: что делать с Featured

| Если | То |
|---|---|
| Featured пустой → | Создать 1 P0-контент (например, case study PDF) в первую неделю |
| 5+ элементов → | Удалить все P2, оставить топ-3 по score |
| Новый P0 появился → | Добавить на первое место, сдвинуть остальные |
| Контент старше 2 лет → | Пересмотреть: если не актуально — удалить |
| Engagement на Featured-посте упал → | Заменить на более свежий или более специфичный |
| Нет ни одного P0-контента → | Создать case study PDF из существующего проекта (1–2 страницы, метрики, архитектура) |

---

## Summary: Section 1 Checklist

- [ ] Headline: 5 вариантов сгенерированы, выбран 1, мобильный preview проверен
- [ ] About: 250–350 слов, PAS-структура, 5 keywords оптимизированы
- [ ] Skills: 20 skills ранжированы по алгоритму, endorsements запрошены
- [ ] Experience: все bullets в STAR, все содержат метрики, defense scripts готовы
- [ ] Embellishment policy: все claims проверены, сомнительные — удалены или смягчены
- [ ] Featured: 3–5 элементов, первый — сильнейший, все <2 лет

**Next step:** Секция 2 — Сеть: стратегия коннекшнов (Section 2 — Network).
# Секция 2 — Meta-расписание запуска инструментов

> **Назначение:** Не расписание контента, а расписание запуска инструментов/промптов. Каждое действие символически ссылается на промпт (P1–P12) или системный промпт (S1–S3). Время указано в Almaty (UTC+5). Предполагается, что весь контент генерируется через промпты и отправляется вручную.

---

## 2.1 Фаза Foundation (Days 1–14)

**Цель:** Получить алгоритмический trust, заполнить профиль, начать органический рост сети. Для fresh profile: 0 комментариев, 0 постов — только passive consumption + профиль + connection requests [→ repo].

### Расписание по дням недели

| День | Время (Almaty) | Инструмент / Промпт | Действие | Ожидаемый результат |
|------|----------------|---------------------|----------|---------------------|
| **Пн** | 08:00–09:00 | Headline Generator + About Generator | Сгенерировать и вручную внедрить headline, about, keywords | Профиль заполнен на 100% [→ repo] |
| | 12:00–12:30 | S2 (Lead Scoring) | Отфильтровать 35 целевых профилей на неделю | Список из 35 prospects с оценкой ≥60 |
| | 18:00–18:30 | P1 (Connection Note Generator) | Отправить 5 персонализированных connection requests (<250 chars, mobile-friendly) [→ repo] | 5 pending invites |
| **Вт** | 08:00–08:30 | S1 (Boolean Search Generator) | Проверить tone-of-voice профиля на consistency | Профиль единообразен |
| | 12:00–12:30 | P1 | Отправить 5 connection requests | 10 cumulative pending |
| | 18:00–18:30 | P12 (SSI Analyzer) — *если конец недели* | Подвести итоги: acceptance rate, SSI delta, profile views | Базовые метрики зафиксированы |
| **Ср** | 08:00–09:00 | About Generator — ревизия | Доработать About на основе PAS + keywords | About оптимизирован |
| | 12:00–12:30 | P1 | Отправить 5 connection requests | 15 cumulative pending |
| | 18:00–18:30 | Passive Consumption | Просмотр 20–30 лент, лайки (без комментариев) [→ repo] | Algorithmic trust растёт |
| **Чт** | 08:00–08:30 | S2 (Lead Scoring) | Добавить 10–15 новых prospects в pipeline | Пополнена воронка |
| | 12:00–12:30 | P1 | Отправить 5 connection requests | 20 cumulative pending |
| | 18:00–18:30 | S3 (Content Calendar) | Проверить, не превышен ли soft cap pending invites (500–700) [→ repo] | Риск restriction минимален |
| **Пт** | 08:00–08:30 | Headline Generator + About Generator | Финальная полировка headline, banner, featured | Профиль готов к public view |
| | 12:00–12:30 | P1 | Отправить 5 connection requests | 25 cumulative pending |
| | 18:00–18:30 | Passive Consumption | Просмотр ленты, лайки, bookmarking | Engagement signals для алгоритма |
| **Сб** | — | — | **Выходной. Нет активных действий.** | Восстановление, предотвращение automation pattern [→ repo] |
| **Вс** | 18:00–18:30 | P12 (SSI Analyzer) | Сводка: новые connections, acceptance rate, SSI, profile views | Чёткая картина для корректировки |

### Foundation Volume Targets

| Метрика | Недельный лимит | Источник |
|---------|-----------------|----------|
| Connection requests | 35 (5/день) | SSI <40 → 50–75/week [→ repo]; conservative start |
| Comments | 0 | Fresh profile = no comments [→ repo] |
| Posts | 0 | Fresh profile = no posts [→ repo] |
| DMs | 0 | Только auto-accept, no outreach |
| Profile views (passive) | 200–300/день | Targeted profile view strategy [→ repo] |

---

## 2.2 Фаза Growth (Days 15–45)

**Цель:** Увеличить acceptance rate до 30–40%, начать комментировать (3/день), публиковать (1/неделю), тёплые DMs (2/неделю) [→ repo]. SSI растёт → лимиты растут.

### Расписание по дням недели

| День | Время (Almaty) | Инструмент / Промпт | Действие | Ожидаемый результат |
|------|----------------|---------------------|----------|---------------------|
| **Пн** | 08:00–09:00 | P4 (Comment Generator) + S2 (Lead Scoring) | Найти 3 поста целевой аудитории, сгенерировать 3 комментария (40+ слов, loop-back method) [→ repo] | 3 comments published |
| | 12:00–12:45 | P1 (Connection Note Generator) | Отправить 10 персонализированных requests (70/неделю) | 10 pending invites |
| | 18:00–18:30 | P2 (Follow-Up DM Generator) | Сгенерировать value-first follow-up для новых acceptances | 2–3 follow-up messages в очереди |
| **Вт** | 08:00–08:30 | P4 | Оставить 3 комментария | 6 comments cumulative |
| | 12:00–12:45 | P1 | Отправить 10 connection requests | 20 pending |
| | 18:00–18:30 | P2 | Отправить follow-up (Day 3–4) для вчерашних acceptances | Тёплый touch, no pitch |
| **Ср** | 08:00–09:00 | P10 (Full Post Generator) | Сгенерировать черновик поста (800–1000 слов, hook, framework, soft CTA) [→ repo] | Черновик готов к review |
| | 12:00–12:45 | P1 | Отправить 10 connection requests | 30 pending |
| | 18:00–18:30 | P4 | Оставить 3 комментария | 9 comments cumulative |
| **Чт** | 08:00–08:30 | P9 (Content Mix Rotator) | Запланировать темы на следующую неделю (без конкретных заголовков — только тематические вёдра) | Content pipeline на 1 неделю вперёд |
| | 12:00–12:45 | P1 | Отправить 10 connection requests | 40 pending |
| | 18:00–18:30 | P2 | Отправить soft CTA (Day 7–10) для старых acceptances | Soft pitch или resource share |
| **Пт** | 08:00–08:30 | P10 | Финальная редакция поста, подготовка к публикации (PDF/карусель если возможно — 1.9x performance [→ repo]) | Пост готов к публикации |
| | 12:00–12:45 | P1 | Отправить 10 connection requests | 50 pending |
| | 18:00–18:30 | P4 | Оставить 3 комментария | 12 comments cumulative |
| **Сб** | — | — | **Выходной. Нет активных действий.** | Предотвращение pattern detection [→ repo] |
| **Вс** | 10:00–10:30 | P10 | **Публикация поста** (1/неделю) | Пост в ленте |
| | 18:00–19:00 | P12 (SSI Analyzer) | Сводка: connections, acceptance rate, SSI, comments, impressions, saves | Data для decision framework |

### Growth Volume Targets

| Метрика | Недельный лимит | Источник |
|---------|-----------------|----------|
| Connection requests | 70 (10/день) | SSI 40–70 → 75–150/week [→ repo]; conservative mid-range |
| Comments | 21 (3/день) | Strategic commenting = 1.8x reach boost [→ repo] |
| Posts | 1 | 2x/week = up to 5x profile views [→ repo]; start with 1 |
| DMs (value-first) | 2 | Warm DM protocol: comment 2x before connect [→ repo] |
| Profile views | 300–500/день | Targeted view strategy [→ repo] |

---

## 2.3 Фаза Conversion (Days 46–90)

**Цель:** Переход от volume к quality. 5 качественных conn/day, 5 comments/day, 5 DMs/week с soft CTAs. Фокус на discovery calls и interviews.

### Расписание по дням недели

| День | Время (Almaty) | Инструмент / Промпт | Действие | Ожидаемый результат |
|------|----------------|---------------------|----------|---------------------|
| **Пн** | 08:00–09:00 | P4 (Comment Generator) | 5 комментариев (40+ слов, high-value, loop-back) [→ repo] | 5 comments published |
| | 12:00–12:30 | S2 (Lead Scoring) + P1 (Connection Note Generator) | Отфильтровать 25 prospects, отправить 5 высококачественных requests | 5 targeted pending invites |
| | 18:00–19:00 | P2 (Follow-Up DM Generator) | Сгенерировать 5-step sequence для hot leads (score ≥80) | Sequence готов |
| **Вт** | 08:00–09:00 | P4 | 5 комментариев | 10 comments cumulative |
| | 12:00–12:30 | P1 | 5 connection requests (качественные, с контекстом из комментариев) | 10 pending |
| | 18:00–18:30 | P2 | Отправить Day 3–4 value message для вчерашних acceptances | Trust building |
| **Ср** | 08:00–09:00 | P10 (Full Post Generator) | Сгенерировать пост или PDF carousel (1.9x performance [→ repo]) | Черновик готов |
| | 12:00–12:30 | P4 | 5 комментариев | 15 comments cumulative |
| | 18:00–18:30 | P2 | Day 7–10 soft CTA для старых acceptances | Soft pitch или resource share |
| **Чт** | 08:00–09:00 | P9 (Content Mix Rotator) + P12 (SSI Analyzer) | Планирование контента + outline newsletter (если аудитория >500) [→ repo] | Pipeline на 2 недели |
| | 12:00–12:30 | P1 | 5 connection requests | 15 pending |
| | 18:00–18:30 | P2 | Day 14–30 follow-up или breakup message | Close или nurture list [→ repo] |
| **Пт** | 08:00–09:00 | P4 | 5 комментариев | 20 comments cumulative |
| | 12:00–12:30 | P1 | 5 connection requests | 20 pending |
| | 18:00–18:30 | P6 (DM to Hiring Manager) | Генерация InMail / direct pitch для hot leads (если Premium) | InMail ready |
| **Сб** | — | — | **Выходной. Нет активных действий.** | Предотвращение pattern detection [→ repo] |
| **Вс** | 10:00–10:30 | P10 | **Публикация поста** (1/неделю, возможно 2/неделю если capacity позволяет) | Пост + engagement |
| | 18:00–19:00 | P12 (SSI Analyzer) | Полный анализ: connections, acceptance rate, reply rate, SSI, calls booked | Data для conversion optimization |

### Conversion Volume Targets

| Метрика | Недельный лимит | Источник |
|---------|-----------------|----------|
| Connection requests | 25 (5/день, качественные) | SSI 70+ → 150–200+/week [→ repo]; но focus = quality, не volume |
| Comments | 35 (5/день) | Comments count 2x as much as likes в алгоритме [→ repo] |
| Posts | 1–2 | 2x/week = 5x profile views [→ repo] |
| DMs (conversion) | 5 | Soft CTA, resource share, discovery call booking |
| Discovery calls | 2–4/неделю | Цель: 15–25 calls/month [→ inferred] |

---

## 2.4 Символическая карта промптов (P1–P12, S1–S3)

| ID | Название | Где используется | Источник |
|----|----------|------------------|----------|
| — | Headline Generator | Foundation, Growth | [→ inferred] |
| — | About Generator | Foundation, Growth | [→ repo] |
| P1 | Connection Note Generator | Foundation, Growth, Conversion | [→ inferred] |
| P2 | Follow-Up DM Generator | Growth, Conversion | [→ repo] |
| P3 | Recruiter Message Generator | Growth, Conversion | [→ inferred] |
| P4 | Comment Generator | Growth, Conversion | [→ inferred] |
| P5 | Post Hook Generator | Growth, Conversion | [→ inferred] |
| P6 | DM to Hiring Manager | Conversion | [→ inferred] |
| P7 | Format Decision Engine | Growth, Conversion | [→ inferred] |
| P8 | Tone & Angle Selector | Growth, Conversion | [→ inferred] |
| P9 | Content Mix Rotator | Growth, Conversion | [→ inferred] |
| P10 | Full Post Generator | Growth, Conversion | [→ repo] |
| P11 | Performance Analyzer | Foundation, Growth, Conversion | [→ inferred] |
| P12 | SSI Analyzer | Foundation, Growth, Conversion | [→ repo] |
| S1 | Boolean Search Generator (Posts) | Все фазы | [→ inferred] |
| S2 | Engagement Scoring / Lead Scoring | Все фазы | [→ repo] |
| S3 | Content Calendar / Topic Discovery | Все фазы | [→ inferred] |

---

## 2.5 Decision Framework — Ежедневная приоритизация

> Правило: **Никогда не запускать всё подряд. Каждое утро — 5-минутный аудит метрик → решение, что запускать сегодня.**

### 5-минутный утренний чек-лист (ежедневно, 07:50–07:55 Almaty)

1. Проверить acceptance rate за последние 7 дней.
2. Проверить SSI динамику (рост / стагнация / падение).
3. Проверить количество pending invites (hard cap ~1,500, soft flags 500–700) [→ repo].
4. Проверить количество booked discovery calls / interviews на неделе.
5. Принять решение по приоритетам на день.

### Decision Rules (если X → то Y)

| Условие (X) | Действие (Y) | Источник |
|-------------|--------------|----------|
| **Acceptance rate > 40%** | → Увеличить connection requests на +5/день (до верхней границы SSI-tier) [→ repo] | [→ repo] |
| **Acceptance rate 25–40%** | → Оставить текущий volume, усилить personalization (P3 depth +1) | [→ inferred] |
| **Acceptance rate < 20%** | → Снизить connection requests на 50%, переключиться на commenting (P5) для warm-up [→ repo] | [→ repo] |
| **SSI вырос на +5+ за неделю** | → Увеличить posting frequency с 1 до 2/неделю (если capacity позволяет) | [→ inferred] |
| **SSI стагнирует 2+ недели** | → Аудит профиля (P1, P2) + усилить commenting (P5) до upper limit фазы | [→ inferred] |
| **SSI падает** | → Полностью остановить connection requests на 3–5 дней, focus на comments и passive consumption | [→ inferred] |
| **Pending invites > 500** | → Приостановить connection requests до снижения <400 (soft flag avoidance) [→ repo] | [→ repo] |
| **Pending invites > 700** | → Остановить connection requests, начать withdraw stale (>3 недель) [→ repo] | [→ repo] |
| **3+ interview / discovery call на неделе** | → Connection requests = 0 на этой неделе, фокус на DM (P4, P9) и follow-up [→ inferred] | [→ inferred] |
| **0 replies на DMs за 2 недели** | → Аудит message tone (S1), увеличить value-first ratio, снизить CTA aggressiveness [→ repo] | [→ repo] |
| **Post saves > 10 за 48h** | → Удвоить commenting на авторах похожего контента (P5 + P8) | [→ inferred] |
| **Profile views < 100/неделю** | → Активировать profile view strategy (200–300 targeted views/день) + проверить headline (P1) [→ repo] | [→ repo] |
| **Restriction warning от LinkedIn** | → Немедленно остановить все automated действия, перейти на 100% manual на 7 дней [→ repo] | [→ repo] |
| **Ban / restriction история (previous_restrictions = true)** | → Conservative mode: max 3 conn/day, 0 automation, manual only [→ repo] | [→ repo] |

---

## 2.6 Meta-правила расписания

1. **Выходной — обязателен.** Суббота = 0 действий. Это не лень, это защита от pattern detection (23% ban rate при aggressive automation [→ repo]).
2. **Ручная отправка всего.** Даже если контент сгенерирован промптом (P6, P3, P4), отправка — ручная. Автоматизация отправки = HIGH risk [→ repo].
3. **Rolling 7-day window.** LinkedIn смотрит на volume за последние 7 дней, не на календарную неделю [→ repo]. Если в понедельник было 15 requests, во вторник — не более 10 (при лимите 25/день).
4. **Almaty timezone (UTC+5).** Все внутренние дедлайны — Almaty time. LinkedIn показывает метки в локальном времени получателя; отправка в 12:00 Almaty = 09:00 Москва, 14:30 Дели — оптимальные B2B часы.
5. **Персонализация > Volume.** Каждый connection request через P3. Каждый comment через P5. Generic = 10–15% acceptance; personalized = 30–40%+ [→ repo].
6. **Content capacity ceiling.** Если `content_capacity` < 3 часов/неделю → посты 1/неделю, без newsletter. Если ≥ 5 часов → можно 2 поста + newsletter outline (P12) [→ repo].
7. **Stop conditions:** 4–5 touches с no response → nurture list, re-approach через 3–6 месяцев [→ repo].

---

## 2.7 Индикаторы перехода между фазами

| Переход | Условие | Проверка |
|---------|---------|----------|
| Foundation → Growth (Day 15) | Профиль 100% + 150+ connections + acceptance rate ≥20% | P11 + P12 |
| Growth → Conversion (Day 46) | 300+ connections + SSI ≥40 + acceptance rate ≥30% + ≥1 post опубликован | P11 + P12 |
| Emergency Revert (любая фаза) | Restriction warning или SSI падение >10 за неделю | Мгновенный revert к Conservative mode |

---

*Source tagging: [→ repo] = извлечено из репозитория linkedin-growth-research; [→ inferred] = логический вывод на основе данных репозитория; [→ web] = внешний источник (не использовано в этой секции).*
# Секция 3 — Discovery Algorithms (Алгоритмы Поиска Целей)

> **Ядро GOS.** Это не список инструментов, а набор воспроизводимых алгоритмов, которые динамически находят цели для комментирования, нетворкинга и контент-идей. Каждый алгоритм параметризован под `{persona}` = Data Engineer, `{geography}` = Almaty/Kazakhstan, `{target_industry}` = Tech.
> 
> **Authority:** repo > web > inference. Все теги источников явны.

---

## 3.1 Algorithm S1: Поиск постов для стратегического комментирования

**Цель:** Находить посты, где комментарий даст максимальный алгоритмический и социальный ROI. Комментарий 40+ слов на посте целевой аудитории = 1.8x reach boost [→ repo].

### 3.1.1 Boolean Strings для поиска

**S1-A. LinkedIn Native Search (бесплатно)**

```
("data engineering" OR "data pipeline" OR "ETL" OR "Apache Spark" OR "PySpark")
AND ("challenge" OR "question" OR "how do you" OR "struggling with" OR "lesson learned")
AND NOT ("hiring" OR "we're looking" OR "job opening" OR "apply now")
```

**S1-B. Google X-Ray (обходит алгоритмическую фильтрацию LinkedIn)**

```
site:linkedin.com/posts/
("data engineering" OR "data infrastructure" OR "Apache Airflow")
("comment" OR "thoughts" OR "what do you think")
-after:2025-01-01
```

**S1-C. Персонализированный Boolean Builder Prompt**

```
Ты — Boolean String Builder для LinkedIn. Построй поисковую строку для нахождения постов для комментирования.

**Входные параметры:**
- Профессия: {persona} (например, "Data Engineer")
- Ключевые навыки: {skills} (например, "Python, SQL, PySpark, Hadoop")
- Целевая аудитория: {target_audience} (например, "Data Engineers, Engineering Managers в fintech")
- Тип поста: {post_type} ("challenge", "question", "story", "technical discussion")
- Исключить: {exclude_terms} ("hiring", "promotion", "course advertisement")

**Правила построения:**
1. Используй операторы: AND, OR, NOT, quotes для exact match
2. Headline — самое тяжеловесное поле в поиске LinkedIn [→ repo]
3. Ограничивай по дате: posts за последние 7 дней ( engagement выше на 60% ) [→ inferred]
4. Добавь вариации с ошибками: "Airflow" OR "Apache Airflow" OR "air flow"
5. Если target_audience = "hiring managers" → добавь seniority keywords: "Director", "Head of", "VP"

**Формат выхода:**
```
LinkedIn Native: <string>
Google X-Ray: <string>
Пояснение: <почему выбраны эти термины>
```
```

### 3.1.2 Критерии отбора постов (Scoring Rubric)

| Критерий | Порог | Вес | Почему |
|----------|-------|-----|--------|
| **Количество комментариев** | < 50 | 30% | Меньше шума → выше видимость комментария [→ inferred] |
| **Подключения автора** | 500–10,000 | 20% | Не микро-аккаунт, не influencer (50k+ перетягивает внимание) [→ inferred] |
| **Активность автора** | Постил за последние 14 дней | 15% | Активные авторы отвечают на комментарии, что запускает треды [→ repo] |
| **Релевантность аудитории** | Автор или его аудитория = target persona | 20% | Комментарий видят люди из твоего ICP [→ repo] |
| **Тип контента** | Вопрос, challenge, "hot take", урок | 15% | Провоцирует дискуссию → алгоритм поднимает комментарий [→ repo] |

**Decision Framework:**
- Если score ≥ 80 → Комментировать в первые 30 минут после публикации [→ inferred]
- Если score 60–79 → Добавить в очередь на сегодня
- Если score < 60 → Пропустить, даже если тема релевантна
- Если автор = influencer (50k+ connections) AND post > 100 comments → Не комментировать, потеряешься в шуме [→ inferred]

### 3.1.3 AI-фильтрация (Prompt для GPT/Claude)

```
Ты — LinkedIn Post Qualifier. Оцени пост по шкале 0–100 для стратегического комментирования.

**Вход:**
- Текст поста: {post_text}
- Автор: {author_headline} (Headline — самое важное поле [→ repo])
- Количество connections автора: {author_connections}
- Количество существующих комментариев: {comment_count}
- Дата публикации: {post_date}

**Критерии оценки (0–100):**
1. **Engagement Potential (0–30):** Пост провоцирует дискуссию? Есть вопрос, challenge, или спорное утверждение?
2. **Audience Match (0–25):** Аудитория автора пересекается с {target_persona} = Data Engineer / Almaty?
3. **Visibility Window (0–20):** Комментариев < 50 AND пост < 24 часов?
4. **Author Accessibility (0–15):** Автор отвечает на комментарии? (проверь по истории)
5. **Content Quality (0–10):** Пост содержит конкретный кейс, данные, или фреймворк?

**Decision Rules:**
- Если score ≥ 80 → Верни: "COMMIT | Priority: HIGH | Reason: ..."
- Если score 60–79 → Верни: "QUEUE | Priority: MEDIUM | Reason: ..."
- Если score < 60 → Верни: "SKIP | Reason: ..."

**Формат:** JSON с полями: score, decision, priority, reasoning, suggested_comment_angle
```

---

## 3.2 Algorithm S2: Поиск людей для нетворкинга

**Цель:** Строить целевой граф связей из 500+ релевантных connections за 90 дней [→ repo]. Приём: 30–40% acceptance rate при персонализированном invite [→ repo] vs 10–15% generic.

### 3.2.1 Boolean Strings для поиска людей

**S2-A. Коллеги и пиры (Peer Network)**

```
("Data Engineer" OR "Data Infrastructure Engineer" OR "Analytics Engineer")
AND ("Python" OR "SQL" OR "PySpark" OR "Hadoop" OR "dbt")
AND ("Almaty" OR "Kazakhstan" OR "Central Asia" OR "Remote")
AND NOT ("intern" OR "junior" OR "student" OR "looking for opportunities")
```

**S2-B. Рекрутеры и Talent Acquisition**

```
("Technical Recruiter" OR "Talent Acquisition" OR "Head of Talent")
AND ("data" OR "engineering" OR "analytics" OR "infrastructure")
AND ("Almaty" OR "Kazakhstan" OR "Astana" OR "Remote EMEA")
AND ("hiring" OR "building team" OR "growing team")
```

**S2-C. Hiring Managers (Engineering Managers, Directors)**

```
("Engineering Manager" OR "Head of Engineering" OR "Director of Data" OR "VP Engineering")
AND ("data" OR "platform" OR "infrastructure")
AND ("Almaty" OR "Kazakhstan" OR "fintech" OR "e-commerce" OR "SaaS")
AND NOT ("consultant" OR "freelance" OR "advisor")
```

**S2-D. Boolean String Builder Prompt (люди)**

```
Ты — Boolean String Builder для поиска людей в LinkedIn Sales Navigator / native search.

**Входные параметры:**
- Цель нетворкинга: {networking_goal} ("peer_learning", "job_opportunities", "partnerships", "mentorship")
- Целевая роль: {target_role} (например, "Engineering Manager")
- География: {geography} ("Almaty", "Kazakhstan", "Remote EMEA", "Global")
- Индустрия: {target_industry} ("fintech", "e-commerce", "SaaS", "consulting")
- Синьорити: {seniority} ("entry", "mid", "senior", "lead", "executive")
- Компании: {target_companies} (список или "any")
- Исключить: {exclude_roles} ("sales", "marketing", "non-technical")

**Правила построения:**
1. Для {networking_goal} = "peer_learning" → включай синьорити ±1 от текущего уровня
2. Для {networking_goal} = "job_opportunities" → фокус на "hiring", "growing team", "we're expanding"
3. Для {geography} = "Almaty" → добавь: "Kazakhstan", "Central Asia", "CIS", "Remote" (многие DE в KZ работают remote)
4. Headline поиск → используй exact match в кавычках [→ repo]
5. Если {target_industry} = "fintech" → добавь: "Kaspi", "ForteBank", "HomeBank.kz" (крупнейшие работодатели DE в KZ) [→ inferred]

**Формат выхода:**
```
LinkedIn Native: <string>
Sales Navigator: <string>
Google X-Ray (resumes): <string>
Пояснение: <почему выбраны эти фильтры>
```
```

### 3.2.2 Критерии отбора людей (Lead Scoring Rubric)

Базовая рубрика из [→ repo] адаптирована под DE/Almaty:

| Критерий | Вес | Порог для Data Engineer / Almaty | Как проверить |
|----------|-----|----------------------------------|---------------|
| **Title/Role match** | 25 | Data Engineer, DE, Analytics Engineer, Platform Engineer | Headline search |
| **Geography match** | 20 | Almaty, Kazakhstan, Remote, EMEA | Location field |
| **Company relevance** | 15 | Финтех, e-com, SaaS, big tech с офисом в KZ | Company page |
| **Activity level** | 15 | Постил за последние 30 дней ИЛИ комментировал | Activity section |
| **Network size** | 10 | 500+ connections (социальный proof) | Connection count |
| **Mutual connections** | 10 | ≥1 mutual = тёплый вход | Mutual connections |
| **Content engagement** | 5 | Посты на технические темы, не только reshares | Recent posts |

**Decision Framework:**
- Если score ≥ 80 → Warm DM protocol: 2 комментария → connection request [→ repo]
- Если score 60–79 → Direct connection request с персонализацией
- Если score 40–59 → Добавить в nurture list, комментировать посты 2 недели, потом connect
- Если score < 40 → Пропустить
- Если mutual connections ≥ 5 AND score ≥ 60 → Попросить взаимного знакомого о intro [→ inferred]
- Если человек = recruiter AND company = target → Автоматически score +15 bonus [→ inferred]

### 3.2.3 Персонализация Connection Request

**Decision Tree по типу цели:**

| Тип цели | Подход | Message шаблон |
|----------|--------|----------------|
| Peer (Data Engineer) | Common ground | "Hi {name}, saw your post on {topic}. Also working with PySpark in Almaty — would love to connect and exchange notes." |
| Recruiter | Value-first | "Hi {name}, your company's data stack looks solid. Happy to share my experience with {tech} if useful for your pipeline." |
| Hiring Manager | Contextual | "Hi {name}, enjoyed your perspective on {topic}. Building data infra at {company} — would love to connect." |
| Alumni / Same city | Geo-bond | "Hi {name}, fellow data engineer in Almaty. Would be great to connect and maybe grab coffee someday." |

**Критические правила:**
- Сообщение < 250 characters (mobile-friendly) [→ repo]
- Никакого pitch в первом сообщении [→ repo]
- Ссылаться на конкретный пост или achievement автора [→ inferred]

---

## 3.3 Algorithm S3: Поиск горячих тем для постов

**Цель:** Находить темы с высоким engagement potential, которые ещё не перегреты конкурентами. Only 1% of 1B users post regularly [→ repo] — возможность в топе.

### 3.3.1 Методология Desktop Research

**S3-A. LinkedIn Trending Topics (нативный)**

1. Открыть LinkedIn → Search → "Trending in {target_industry}"
2. Фильтр: Posts → Last 7 days → Sort by relevance
3. Поисковые строки:
   ```
   "data engineering" + "trend"
   "Apache Spark" + "2025" OR "2026"
   "data pipeline" + "challenge" OR "lesson"
   ```
4. Анализ: какие темы получают >100 reactions за 24 часа у авторов < 50k followers?

**S3-B. LinkedIn Newsletters Deep Research**

1. Подписаться на 10–15 Newsletters в нише:
   - Data Engineering Newsletter (Joe Reis)
   - Analytics Engineering Roundup
   - ByteByteGo System Design
2. Мониторить: какие темы повторяются в разных newsletters за неделю?
3. Если тема упомянута ≥3 newsletters AND твой угол уникален → высокий приоритет [→ inferred]

**S3-C. Google Trends (бесплатно)**

```
URL: https://trends.google.com/trends/explore
Query: "data engineering", "Apache Spark", "PySpark", "dbt", "data mesh"
Geo: Kazakhstan, Worldwide
Time: Past 90 days
```

**Decision Framework:**
- Если Google Trends показывает рост >20% за 90 дней AND мало постов на LinkedIn → **High-priority topic** [→ inferred]
- Если Trends плоский BUT LinkedIn активность высокая → **Saturation risk**, ищи sub-niche
- Если Trends растёт AND LinkedIn активность низкая → **First-mover advantage**, пиши немедленно [→ inferred]

**S3-D. Competitor Content Gap Analysis**

**Алгоритм:**
1. Составить список 10–15 DE creators с 5k–50k followers (твой aspiration tier)
2. Скрейпнуть их посты за последние 90 дней (ручной или через n8n RSS)
3. Категоризировать: какие темы они НЕ покрывают?
4. Проверить: какие вопросы задают в комментариях к их постам?

### 3.3.2 Content Gap Prompt

```
Ты — Content Gap Analyst для LinkedIn. Найди темы для постов, которые пересекаются с экспертизой автора, но недостаточно покрыты конкурентами.

**Входные параметры:**
- Экспертиза автора: {author_expertise} ("Python, SQL, PySpark, Hadoop, Airflow, data pipelines")
- География автора: {author_geography} ("Almaty, Kazakhstan")
- Целевая аудитория: {target_audience} ("Data Engineers, Engineering Managers")
- Конкуренты (список постов за 90 дней): {competitor_posts}
- Тренды в индустрии: {industry_trends} (из Google Trends, newsletters)

**Задача:**
1. Проанализируй {competitor_posts}: какие темы перегреты (≥3 поста на ту же тему)?
2. Проанализируй {industry_trends}: какие тренды автор может объяснить через свой опыт?
3. Найди пересечение: что автор знает лучше конкурентов AND что рынок ищет?

**Формат выхода:**
```json
{
  "saturated_topics": ["topic1", "topic2"],
  "gap_opportunities": [
    {
      "topic": "...",
      "angle": "уникальный угол автора",
      "evidence": "почему это gap (данные)",
      "priority": "HIGH/MEDIUM/LOW",
      "format": "text post / carousel / video"
    }
  ],
  "personal_experience_gaps": ["темы из опыта автора, не освещённые конкурентами"]
}
```

**Decision Rules:**
- Если priority = HIGH AND evidence содержит числа → Писать в ближайшие 7 дней
- Если topic = пересечение тренда и локального опыта (например, "Data Engineering в Казахстане: что не так с локальными банками") → Автоматически HIGH [→ inferred]
```

### 3.3.3 Персонализированные темы для Data Engineer / Almaty

**Если X → то Y фреймворк для выбора тем:**

| Если (условие) | То (действие) | Пример |
|----------------|---------------|--------|
| Тема = global trend AND локальный angle отсутствует у конкурентов | Писать немедленно, first-mover | "Data Mesh в Kaspi Bank: реальность vs хайп" |
| Тема = частый вопрос в комментариях к чужим постам | Писать FAQ-пост или carousel | "PySpark vs Pandas: когда что использовать" |
| Тема = личный failure + lesson learned | Писать story-led post | "Как я уронил production pipeline в пятницу вечером" |
| Тема = инструмент, который все обсуждают, но мало кто использовал | Писать hands-on review | "3 месяца с dbt: честный отзыв из KZ" |
| Тема = перегрета (≥5 постов за неделю от крупных авторов) | Пропустить или найти sub-niche | Вместо "Что такое Data Mesh" → "Data Mesh в компаниях < 100 человек" |

---

## 3.4 Tool Reference Guide (Справочник инструментов)

> **Принцип:** Инструменты — это не цель, а реализация алгоритмов. Каждый алгоритм (S1, S2, S3) может работать вручную; инструменты масштабируют.

### 3.4.1 Sales Navigator

| Атрибут | Значение |
|---------|----------|
| **URL** | https://www.linkedin.com/sales |
| **Free tier** | 1-month free trial (требует кредитную карту) [→ web] |
| **Paid** | Core: $99/mo; Advanced: $149/mo; Advanced Plus: custom [→ web] |
| **Kazakhstan** | Доступен; требует LinkedIn Premium. Оплата возможна международной картой. Нет явных geo-ограничений [→ inferred] |
| **Использование в GOS** | S2 (поиск людей): Boolean search, lead lists, InMail. S1 (посты): поиск по хэштегам и keywords с фильтрами |
| **Risk** | SAFE (официальный инструмент LinkedIn) [→ repo] |
| **Альтернатива бесплатно** | LinkedIn native search + Google X-Ray + ручная фильтрация |

### 3.4.2 Google Alerts

| Атрибут | Значение |
|---------|----------|
| **URL** | https://www.google.com/alerts |
| **Free tier** | Полностью бесплатно, неограниченное число alerts [→ web] |
| **Kazakhstan** | Полностью доступен [→ inferred] |
| **Использование в GOS** | S3 (темы): алерты на "data engineering trends", "Apache Spark updates", имени конкурентов. S2 (люди): алерты на hiring announcements целевых компаний |
| **Risk** | SAFE |
| **Setup example** | ```Query: "data engineering" AND ("Kazakhstan" OR "Almaty") AND ("hiring" OR "team")``` |

### 3.4.3 RSS + Feed Readers

| Атрибут | Значение |
|---------|----------|
| **URL** | Feedly (https://feedly.com), Inoreader (https://www.inoreader.com) |
| **Free tier** | Feedly: 100 sources, 3 feeds бесплатно; Inoreader: бесплатный план с базовыми фичами [→ web] |
| **Kazakhstan** | Доступны [→ inferred] |
| **Использование в GOS** | S3 (темы): агрегация newsletters, блогов (Airbnb Engineering, Netflix Tech Blog, etc.), Reddit r/dataengineering. S1 (посты): RSS LinkedIn через n8n для автоматического сбора постов |
| **Risk** | SAFE |
| **Setup** | Создать коллекцию "DE Daily": добавить 20 источников, проверять 1 раз в день |

### 3.4.4 n8n

| Атрибут | Значение |
|---------|----------|
| **URL** | https://n8n.io |
| **Free tier** | Self-hosted: полностью бесплатно, unlimited workflows; Cloud: free plan с ограниченными executions [→ web] |
| **Kazakhstan** | Доступен; self-hosted версия работает на любом сервере/VPS [→ web] |
| **Использование в GOS** | Автоматизация S1: сбор постов по RSS → AI-фильтрация (OpenAI node) → выдача в Telegram/Slack. Автоматизация S3: сбор Google Trends → оповещение о росте. Автоматизация S2: интеграция с LinkedIn (через unofficial API — **ToS-violating, HIGH risk**) [→ repo] |
| **Risk** | SAFE для RSS + AI; HIGH для LinkedIn automation |
| **Decision Framework** | Если risk_tolerance = conservative → использовать n8n только для RSS + alerts. Если risk_tolerance = aggressive → можно LinkedIn scraping через unofficial API с ротацией прокси [→ inferred] |

### 3.4.5 Make (Integromat)

| Атрибут | Значение |
|---------|----------|
| **URL** | https://www.make.com |
| **Free tier** | 1,000 operations/month бесплатно; ограничение по количеству scenarios [→ web] |
| **Kazakhstan** | Доступен; нет geo-ограничений [→ web] |
| **Использование в GOS** | Аналогично n8n: интеграция RSS + Google Sheets + AI (OpenAI module). Более визуальный интерфейс, меньше кода. |
| **Risk** | SAFE для RSS + alerts; HIGH для LinkedIn automation |
| **Decision Framework** | Если budget_monthly = 0 AND tech_skill = low → Make (проще в освоении). Если budget_monthly = 0 AND tech_skill = high → n8n self-hosted (дешевле при масштабе) [→ inferred] |

### 3.4.6 Сводная таблица: Какой инструмент для какого алгоритма

| Алгоритм | Ручной (бесплатно) | Полуавтомат (умеренный бюджет) | Автомат (агрессивный рост) |
|----------|-------------------|-------------------------------|---------------------------|
| **S1: Посты** | LinkedIn native search + Google X-Ray | Feedly RSS + n8n alerts | n8n + unofficial LinkedIn API (HIGH risk) [→ repo] |
| **S2: Люди** | LinkedIn native search + Boolean strings | Sales Navigator trial + ручная квалификация | Sales Navigator + Apollo.io + n8n enrichment |
| **S3: Темы** | Google Trends + Newsletter чтение | Google Alerts + Feedly + n8n aggregation | n8n + custom scraper + AI analysis |

---

## 3.5 Integration: Как Discovery Algorithms встраиваются в Pipeline

**Входы и выходы каждого алгоритма:**

```
Inputs (parameters) → Algorithm S1/S2/S3 → Outputs → Next Stage
```

| Алгоритм | Вход (из Parameterization Schema) | Выход | Следующий этап |
|----------|-----------------------------------|-------|----------------|
| **S1** | `{persona}`, `{skills}`, `{target_audience}`, `{post_type}` | Очередь из 5–10 постов для комментирования | Execution: комментирование 5–10/day |
| **S2** | `{networking_goal}`, `{target_role}`, `{geography}`, `{seniority}` | Список 20–50 квалифицированных лидов | Execution: Warm DM protocol |
| **S3** | `{author_expertise}`, `{competitor_posts}`, `{industry_trends}` | 3–5 тем для постов на 2 недели | Execution: Content Creation |

**Feedback Loops:**
1. **S1 → S2:** Если комментарий на посте автора получил ответ → автор переходит из S1 в S2 как warm lead [→ inferred]
2. **S2 → S3:** Если лид часто постит на определённую тему → тема добавляется в S3 как potential gap или collaboration topic [→ inferred]
3. **S3 → S1:** Если пост автора на тему из S3 получил высокий engagement → тема добавляется в S1 как trending topic для комментирования [→ inferred]

---

## 3.6 Чек-лист ежедневного Discovery Routine

| Время | Действие | Алгоритм | Инструмент |
|-------|----------|----------|------------|
| 08:00 | Проверить RSS/Feedly на новые посты | S1 | Feedly / n8n |
| 08:15 | Проверить Google Alerts | S3 | Google Alerts |
| 08:30 | LinkedIn native search: посты за последние 24 часа | S1 | LinkedIn |
| 09:00 | Поиск 5–10 новых людей по Boolean string | S2 | LinkedIn / Sales Navigator |
| 18:00 | Weekly review: какие посты/темы дали highest engagement? | S1, S3 | Аналитика вручную |

---

## Source Summary

- [→ repo]: Данные из индексов Wave 1 (docs/01–11, pipeline, tools, parameters)
- [→ web]: Внешние источники (цены инструментов, доступность в KZ)
- [→ inferred]: Логические выводы на основе repo + web, адаптированные под Data Engineer / Almaty

---

*Версия: 2026-05-14*
*Следующая секция: Section 4 — Execution Playbooks (алгоритмы действий после discovery)*
# Секция 4: Prompt Library — Промпты для генерации персонализированных сообщений

> **Назначение:** Это не шаблоны сообщений. Это промпты для AI, которые генерируют персонализированные сообщения под конкретную ситуацию. Каждый промпт параметризован и содержит правила генерации, примеры вызова и ограничения.

---

## P1: Connection Note Generator

**Назначение:** Генерация персонализированного текста для запроса на подключение (connection request note).

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `target_name` | string | Имя получателя (только имя, без фамилии) | Да |
| `role` | string | Должность получателя | Да |
| `company` | string | Компания получателя | Да |
| `shared_context` | string | Общий контекст (взаимные связи, группы, география) | Нет |
| `recent_post` | string | Тема недавнего поста получателя (1-2 предложения) | Нет |
| `angle` | enum | Угол подхода: `content`, `mutual`, `industry`, `geo`, `event` | Да |
| `my_role` | string | Ваша роль / позиционирование | Да |
| `my_company` | string | Ваша компания | Нет |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-аутричу. Сгенерируй 3 варианта текста для запроса на подключение.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- Максимум 200 символов (включая пробелы) — мобильный интерфейс обрезает всё остальное
- Только имя получателя ("Привет, {target_name}!"), без фамилии
- Одна конкретная деталь — не "отличный профиль", а "ваш пост о {specific_detail}"
- Добавь ценность ДО просьбы — не "давайте обменяемся опытом", а "ваш кейс с {company} напомнил мне..."
- Никаких ссылок, эмодзи, CTA типа "давайте созвонимся"
- Без шаблонных фраз: "impressive background", "love your work", "let's connect"

УГЛЫ ПОДХОДА (angle):
- content: референс к конкретному посту или комментарию
- mutual: упоминание взаимных связей
- industry: общий индустриальный контекст + ценность
- geo: география ("из Алматы тоже" / "работаю с командами в {region}")
- event: общее событие, конференция, вебинар

СТРУКТУРА (2-3 предложения):
1. Контекст + конкретная деталь
2. Кто вы (1 строка, релевантная получателю)
3. [опционально] Мягкий мост — НЕ CTA

КАЖДЫЙ ВАРИАНТ ДОЛЖЕН ОТЛИЧАТЬСЯ по тону:
- A: Профессиональный, прямой
- B: Контекстуальный, с вопросом
- C: Лёгкий, с персональной ноткой

ФОРМАТ ВЫВОДА:
Вариант A (профессиональный): [текст]
Длина: [X символов]

Вариант B (контекстуальный): [текст]
Длина: [X символов]

Вариант C (лёгкий): [текст]
Длина: [X символов]
```

### Пример вызова (DE из Алматы)

```
target_name: Асель
role: Senior Data Engineer
company: Kaspi Bank
shared_context: обе из Алматы, взаимные связи: Динара (ex-Yandex)
recent_post: "Про оптимизацию Spark-джобов в DWH — уменьшили runtime на 40%"
angle: content
my_role: Data Engineer в продуктовой компании
my_company: Avito
```

**Ожидаемый результат:**
- Вариант А: "Привет, Асель! Ваш пост об оптимизации Spark-джобов — отличный кейс. Сейчас решаю похожую задачу в Avito. Буду рад быть в сети."
- Вариант Б: "Асель, читал ваш пост про Spark — уменьшение runtime на 40% впечатляет. Какой подход оказался ключевым? Было бы полезно быть на связи."
- Вариант В: "Привет, Асель! Ваш кейс с Kaspi вдохновил — мы тоже боремся с DWH-оптимизацией. Из Алматы в Москву с приветом :)"

### Источники правил
- Лимит 200 символов и mobile-friendly: [→ repo]
- Level 2–3 personalization даёт 30–40% acceptance vs 5–10% generic: [→ repo]
- First name only, specific detail, value before ask: [→ repo]

---

## P2: Follow-up DM Generator

**Назначение:** Генерация follow-up сообщений после принятия запроса на подключение.

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `name` | string | Имя получателя | Да |
| `context` | string | Контекст подключения (что было в connection note) | Да |
| `days_since` | integer | Дней с момента подключения | Да |
| `recent_activity` | string | Недавняя активность получателя (пост, job change, проект) | Нет |
| `value_offer` | string | Что вы можете дать (инсайт, ресурс, кейс) | Да |
| `sequence_step` | enum | `day3_4` (value-first), `day7_10` (soft CTA) | Да |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-аутричу. Сгенерируй follow-up сообщение.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- Если days_since = 3-4 (sequence_step = day3_4): ТОЛЬКО ценность, НИКАКОГО CTA, НИКАКОГО питча
- Если days_since = 7-10 (sequence_step = day7_10): Мягкий CTA ("буду рад поделиться", "если интересно")
- Максимум 4-5 коротких предложений
- Никаких ссылок в первом сообщении
- Не упоминай, что "я вижу, вы давно не писали" — это давление
- Не используй фразы: "following up", "circling back", "just checking in"

СТРУКТУРА (day3_4 — value-first):
1. Контекст подключения (1 предложение, напомни, откуда знакомы)
2. Ценность (2-3 предложения: инсайт, релевантный ресурс, мини-кейс)
3. Открытый конец (без CTA — просто делишься)

СТРУКТУРА (day7_10 — soft CTA):
1. Контекст (1 предложение)
2. Ценность (2 предложения)
3. Мягкий CTA (1 предложение, "happy to share if relevant", "могу рассказать подробнее")

КАЖДЫЙ ВЫЗОВ генерирует 2 варианта с разным тоном:
- A: Профессиональный, фактологический
- B: Личный, разговорный

ФОРМАТ ВЫВОДА:
Вариант A: [текст]
Вариант B: [текст]
Sequence step: {sequence_step}
Character count: [X]
```

### Пример вызова (DE из Алматы)

```
name: Асель
context: "Подключились после её поста про Spark-оптимизацию в Kaspi"
days_since: 4
recent_activity: "Недавно запостила про переход с Airflow на Dagster"
value_offer: "Кейс из Avito — миграция 200+ пайплайнов с Airflow, типичные подводные камни"
sequence_step: day3_4
```

**Ожидаемый результат:**
- Вариант А: "Асель, рад быть на связи. Увидел ваш пост про переход на Dagster — актуальная тема. Мы в Avito прошли через миграцию 200+ пайплайнов с Airflow, нашли несколько неочевидных моментов с зависимостями. Если будет полезно — могу поделиться списком."
- Вариант Б: "Привет, Асель! Слежу за вашими постами про data-инфраструктуру — классные кейсы. Кстати, про Dagster: мы недавно прошли через похожую миграцию, было больно, но оно того стоит. Если интересно, расскажу, на что обратить внимание в первую очередь."

### Источники правил
- Day 3–4: value-first, no pitch, no CTA: [→ repo]
- Day 7–10: soft CTA: [→ repo]
- 15–25% reply rate на well-timed messages: [→ repo]

---

## P3: Recruiter Message Generator

**Назначение:** Генерация письма рекрутеру с сопроводительным текстом и subject line.

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `recruiter_name` | string | Имя рекрутера | Да |
| `company` | string | Компания | Да |
| `job_desc` | string | Описание вакансии (2-3 ключевых требования) | Да |
| `highlights` | array | Ваши релевантные достижения (2-3 пункта) | Да |
| `target_role` | string | Название роли | Да |
| `my_experience` | string | Краткое описание опыта (2-3 года, стек) | Да |
| `referral` | string | Реферал (если есть) | Нет |
| `message_type` | enum | `inmail`, `connection_note`, `dm_after_connect` | Да |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-рекрутингу. Сгенерируй сообщение рекрутеру.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- Если message_type = connection_note: < 200 символов, мобильный-френдли
- Если message_type = inmail: 150-250 слов, структурированный
- Если message_type = dm_after_connect: 4-5 предложений, мягкий
- Subject line (для InMail): конкретный, не "Job application" или "Frontend Developer"
- Никаких generic фраз: "I am a highly motivated professional", "proven track record"
- Каждое достижение — с метрикой или контекстом

СТРУКТУРА (InMail):
Subject: [Company] — [Role]: [specific hook, e.g. "3 years in fintech data infra"]

1. Контекст (1-2 предложения: где нашли вакансию, если реферал — упомянуть)
2. Релевантность (2-3 предложения: какой опыт совпадает с требованиями, конкретные достижения с метриками)
3. Ценность (1-2 предложения: что вы принесёте компании, не что хотите получить)
4. CTA (1 предложение: мягкий next step, "буду рад обсудить" / "готов созвониться")

СТРУКТУРА (Connection Note):
1. Контекст (вижу вакансию / реферал)
2. Краткая релевантность (1 достижение с метрикой)
3. Мягкий мост

ФОРМАТ ВЫВОДА:
Тип: {message_type}
Subject: [текст]

Тело письма:
[текст]

Character count / Word count: [X]
```

### Пример вызова (DE из Алматы)

```
recruiter_name: Елена
company: Avito
job_desc: "Senior Data Engineer. Требования: Python, Spark, Airflow/Dagster, опыт с DWH от 3 лет, оптимизация пайплайнов"
highlights: [
  "Уменьшил runtime ETL-пайплайнов на 35% за счёт редизайна Spark-джобов",
  "Мигрировал 200+ пайплайнов с Airflow на Dagster без даунтайма",
  "Построил DWH с нуля для финтеха (10M+ записей/день)"
]
target_role: Senior Data Engineer
my_experience: "4 года в data engineering. Python, Spark, Airflow, Dagster, ClickHouse, Kafka. Последние 2 года в финтехе."
referral: "Реферал от Дмитрия Петрова (ex-коллега)"
message_type: inmail
```

**Ожидаемый результат:**
- Subject: "Senior DE — 4 года в data, миграция 200+ пайплайнов (реферал от Д. Петрова)"
- Тело: "Елена, привет. Дмитрий Петров порекомендовал написать по вакансии Senior Data Engineer. За последние 2 года в финтехе я: (1) сократил runtime ETL на 35% через редизайн Spark-джобов, (2) мигрировал 200+ пайплайнов с Airflow на Dagster без даунтайма, (3) построил DWH с нуля под 10M+ записей/день. Ищу роль, где можно работать с масштабом и оптимизацией. Буду рад обсудить, подходит ли мой опыт."

### Источники правил
- Connection request + personalized note: 30–40% acceptance: [→ repo]
- InMail response rate 10–18%: [→ repo]
- Subject line: конкретный hook повышает open rate: [→ inferred]

---

## P4: Comment Generator

**Назначение:** Генерация осмысленных комментариев к постам (не "Great post!").

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `post_text` | string | Текст поста (или ключевая идея) | Да |
| `author` | string | Имя автора | Да |
| `expertise_tags` | array | Теги вашей экспертизы (пересечение с темой) | Да |
| `tone` | enum | `insightful`, `questioning`, `supportive`, `contrarian` | Да |
| `comment_goal` | enum | `engage`, `warm_up`, `establish_expertise` | Да |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-engagement. Сгенерируй 3 варианта комментария к посту.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- МИНИМУМ 40 слов — one-liner'ы не работают и не индексируются алгоритмом
- НЕ "Great post!", "Thanks for sharing", "Totally agree", "Love this!"
- Добавь ценность: инсайт, контрпример, вопрос, личный опыт, дополнение
- Если tone = contrarian: вежливо, с аргументацией, не троллинг
- Если comment_goal = warm_up: Loop-Back Method (appreciation + bonus value + open question)
- Никаких ссылок на свой контент (выглядит как спам)

СТРУКТУРА (Loop-Back Method для warm_up):
1. Appreciation: конкретно, что понравилось (не "отличный пост")
2. Bonus value: инсайт, кейс, дополнение из вашего опыта
3. Open question: вопрос, который продолжает разговор

ВАРИАНТЫ:
- A: Insightful — инсайт, который дополняет пост
- B: Questioning — вопрос, который углубляет тему
- C: Supportive — личный опыт + подтверждение тезиса

ФОРМАТ ВЫВОДА:
Вариант A (insightful): [текст]
Длина: [X слов]

Вариант B (questioning): [текст]
Длина: [X слов]

Вариант C (supportive): [текст]
Длина: [X слов]
```

### Пример вызова (DE из Алматы)

```
post_text: "Переходим с Airflow на Dagster. Главная боль — 200+ DAGs, которые никто не документировал 3 года."
author: Асель
expertise_tags: ["data engineering", "Airflow", "Dagster", "pipeline migration"]
tone: insightful
comment_goal: warm_up
```

**Ожидаемый результат:**
- Вариант А: "Асель, узнаю ситуацию — мы проходили через то же самое. Наш подход: перед миграцией запустили auto-discovery скрипт, который построил dependency graph из SQL-логов. Обнаружили 30% "мертвых" DAGs, которые вообще не запускались полгода. Сократили scope миграции на треть. Вы тоже делаете аудит перед переходом или мигрируете всё подряд?"
- Вариант Б: "Симпатичная задача. Интересно, как вы подходите к prioritization — мигрируете по критичности пайплайнов или по сложности? Мы сначала взяли revenue-critical, а потом оказалось, что самые сложные — не самые важные. Какой критерий у вас главный?"
- Вариант В: "Асель, спасибо за честность — редко кто пишет про грязную работу миграции. Прошли через похожее: 3 года без документации + bus factor = 1. Ключевое открытие: половина 'сложных' DAGs оказалась копипастой с разными параметрами. Когда выделили шаблоны — процесс ускорился в 2 раза. Держитесь, оно того стоит!"

### Источники правил
- Комментарии 40+ слов, Loop-Back Method: [→ repo]
- Комментарии 2x эффективнее лайков для алгоритма: [→ repo]
- Comments → 5-10x profile views, 3-4x DM response rate: [→ repo]

---

## P5: Post Hook Generator

**Назначение:** Генерация хуков (заголовков-зацепок) для LinkedIn-постов.

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `topic` | string | Тема поста | Да |
| `audience` | string | Целевая аудитория | Да |
| `angle` | enum | Угол: `mistake`, `contrarian`, `story`, `how_to`, `listicle`, `hot_take` | Да |
| `format` | enum | Формат: `text`, `carousel`, `video`, `poll` | Да |
| `trend` | string | Актуальный тренд/контекст (если применим) | Нет |
| `expertise_level` | enum | `beginner`, `intermediate`, `expert` | Да |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-контенту. Сгенерируй 5 хуков для поста.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- Хук должен работать без кликбейта — конкретная ценность в первых 2 строках
- Для формата carousel: хук должен обещать визуальную структуру ("3 фреймворка...", "Пошаговый гайд...")
- Для формата text: хук — текстовый, с цифрами или контринтуитивным тезисом
- Не использовать: "You won't believe", "This is crazy", "Mind-blowing" — триггерят скепсис
- Хук + первые 2 строки должны быть < 300 символов (видно без "...see more")

ТИПЫ ХУКОВ:
1. Pattern Interrupt: "Я потратил 2 года на X. Вот что я понял через 3 дня после того, как бросил."
2. Specific Numbers: "3 ошибки, которые стоят мне $50K в зарплате."
3. Contrarian: "Все говорят делать X. Я делаю противоположное — и вот почему."
4. Story Open: "В 2021 году меня уволили за то, что я отказался..."
5. How-to Promise: "Как я сократил runtime на 40% без добавления серверов."

КАЖДЫЙ ВАРИАНТ включает:
- Hook (заголовок)
- Format suggestion (как оформить пост)
- CTA suggestion (что спросить в конце)

ФОРМАТ ВЫВОДА:
Вариант 1: [hook]
Формат: [suggestion]
CTA: [suggestion]

Вариант 2: ...
```

### Пример вызова (DE из Алматы)

```
topic: "Миграция с Airflow на Dagster"
audience: "Data Engineers и Tech Leads"
angle: mistake
format: carousel
expertise_level: expert
```

**Ожидаемый результат:**
- Вариант 1: "Я мигрировал 200+ пайплайнов с Airflow на Dagster. Вот 5 ошибок, которые стоили нам 3 месяца лишней работы." / Формат: Carousel, 5 слайдов, каждый — ошибка + решение / CTA: "Какую миграцию вы сейчас планируете?"
- Вариант 2: "Все говорят: 'Dagster лучше Airflow'. Никто не говорит про эти 3 подводных камня." / Формат: Text + code snippet / CTA: "Сталкивались с подобным?"
- Вариант 3: "3 года назад мы перешли на Dagster. Через 6 месяцев вернулись к Airflow. Вот почему." / Формат: Story carousel / CTA: "Ваш опыт миграций — удачный или больной?"
- Вариант 4: "Я сэкономил 120 часов dev time на миграции. Вот единственный подход, который сработал." / Формат: Step-by-step carousel / CTA: "Как вы prioritизируете миграции?"
- Вариант 5: "Airflow vs Dagster: честный разбор от человека, который использовал оба в продакшене." / Формат: Comparison table carousel / CTA: "За какой стек вы голосуете — и почему?"

### Источники правил
- PDF carousels 1.9x эффективнее других форматов: [→ repo]
- "See more" клик — ключевой сигнал алгоритму: [→ repo]
- Комментарии 2x лайков для reach: [→ repo]

---

## P6: DM to Hiring Manager

**Назначение:** Генерация сообщения hiring manager'у (не рекрутеру) — холодное или тёплое.

### Входные параметры

| Параметр | Тип | Описание | Обязательный |
|----------|-----|----------|-------------|
| `hm_name` | string | Имя hiring manager'а | Да |
| `company` | string | Компания | Да |
| `department` | string | Департамент/команда | Да |
| `value_prop` | string | Ваша ценность (1-2 предложения с метриками) | Да |
| `referral` | string | Реферал или тёплая связь | Нет |
| `message_temperature` | enum | `cold`, `warm`, `hot` | Да |
| `job_context` | string | Контекст (видели вакансию / нет, но компания интересна) | Да |

### Правила генерации [→ repo]

```
Ты — эксперт по LinkedIn-аутричу. Сгенерируй сообщение hiring manager'у.

ЖЁСТКИЕ ОГРАНИЧЕНИЯ:
- Если cold: 2 предложения max, мягкий, не "нанимайте меня"
- Если warm: можно 3-4 предложения, с референсом к общему контексту
- Если hot: прямой, но вежливый — вы уже знакомы или в процессе
- Никогда не начинай с "I'm looking for a job" или "Are you hiring?"
- Начинай с их проблемы или контекста, не со своих потребностей
- Если нет реферала — найди конкретную деталь (пост, проект, конференция)

СТРУКТУРА (cold):
1. Контекст (1 предложение: их пост / проект / общий контакт)
2. Релевантность (1 предложение: какой опыт релевантен их команде, с метрикой)
3. Мягкий мост (не CTA — "было бы интересно" / "слежу за вашими проектами")

СТРУКТУРА (warm):
1. Контекст (реферал / общий контакт / взаимодействие)
2. Релевантность (2 предложения)
3. Мягкий CTA ("если у вас есть задачи в X — буду рад обсудить")

СТРУКТУРА (hot):
1. Контекст (процесс / знакомство)
2. Ценность (1-2 предложения)
3. Прямой CTA ("когда можно обсудить?" / "готов к следующему этапу")

ФОРМАТ ВЫВОДА:
Cold (2 варианта):
Вариант A: [текст]
Вариант B: [текст]

Warm (1 вариант):
[текст]
```

### Пример вызова (DE из Алматы)

```
hm_name: Данияр
company: Yandex
department: Data Platform
value_prop: "4 года в data engineering. Последний проект: построил DWH с нуля для финтеха, 10M+ записей/день. Сократил runtime ETL на 35%."
referral: null
message_temperature: cold
job_context: "Видел вакансию Senior Data Engineer в вашем департаменте. Слежу за постами команды про миграцию на новый стек."
```

**Ожидаемый результат:**
- Cold А: "Данияр, привет. Слежу за постами вашей команды Data Platform про миграцию стека — интересный кейс. У меня 4 года в DE, последний проект — DWH с нуля под 10M+ записей/день + оптимизация runtime на 35%. Было бы интересно посмотреть, над чем работаете сейчас."
- Cold Б: "Данияр, видел вакансию Senior DE в вашей команде. Параллельно читал ваш пост про миграцию — узнаю многие проблемы. Построил похожую инфраструктуру в финтехе, сократил ETL-runtime на 35%. Если ищете человека с опытом масштабирования — буду рад пообщаться."
- Warm: "Данияр, Асель из Kaspi порекомендовала написать. Увидел, что вы ищете Senior DE на Data Platform. Мой бэкграунд — 4 года в DE, DWH с нуля под 10M+/день, оптимизация пайплайнов на 35%. Если мой опыт релевантен — буду рад обсудить, как могу помочь команде."

### Источники правил
- Warm DM: 3-4x response rate vs cold: [→ repo]
- Connection requests: 30-40% acceptance с Level 2-3 personalization: [→ repo]
- Value-first approach: [→ repo]

---

## Sequencing Decision Framework

### Когда использовать 3-step vs 5-step vs multi-channel

| Фактор | 3-Step (Day 0/3-4/7-10) | 5-Step (Day 1/4/8/15/30) | Multi-Channel (8 точек / 28 дней) |
|--------|------------------------|--------------------------|-----------------------------------|
| **Ценность контакта** | Средняя | Высокая | Очень высокая |
| **Ваш SSI** | < 40 | 40–70 | 70+ |
| **Взаимные связи** | Да | Да / Нет | Нет (cold) |
| **Активность в контенте** | Да | Иногда | Нет |
| **Доступ к email/телефону** | Нет | Иногда | Да |
| **Ресурсы на персонализацию** | 5 мин/контакт | 15 мин/контакт | 30+ мин/контакт |

### Правило выбора [→ repo]

```
ЕСЛИ target = hiring manager В конкретной компании → 5-step или multi-channel
ЕСЛИ target = рекрутер (активно ищет) → 3-step, day3-4 с value, day7-10 с CTA
ЕСЛИ target = peer / потенциальный реферал → 3-step или 2-step warm DM (comments first)
ЕСЛИ target = инфлюенсер / thought leader → comments only, no DM (пока не взаимодействие)
ЕСЛИ у вас < 300 connections → 3-step, максимальный volume
ЕСЛИ у вас 500+ connections → 5-step для high-value, 3-step для остальных
```

### Как определить "высокую ценность" [→ inferred]

| Критерий | Вес | Порог "высокой ценности" |
|----------|-----|------------------------|
| Hiring manager в target company | 3x | Любой |
| Рекрутер с активной вакансией | 2x | Релевантная вакансия < 30 дней |
| Взаимный реферал | 2x | Работал с вами или может порекомендовать |
| Thought leader в вашей нише | 1.5x | 10K+ followers, активен |
| Peer в target company | 1x | Может дать инсайт / реферал |

**Правило:** Если сумма весов ≥ 3 → используй 5-step или warm DM protocol. Если < 3 → 3-step.

### Когда переходить к DM [→ repo]

| Условие | Действие |
|---------|----------|
| 2+ осмысленных комментария к постам | → Можно отправлять connection request |
| Принял connection request | → Day 3-4 value message |
| Ответил на Day 3-4 | → Day 7-10 soft CTA |
| Не ответил на Day 7-10 | → Breakup message или nurture |
| Принял, но не ответил | → Комментируй посты 2 недели, потом повторный DM |
| Взаимные связи + онлайн | → Можно сразу connection request с референсом |
| Cold 3rd degree, нет активности | → Comments first (warm DM protocol) |

### Stop conditions [→ repo]

```
ЕСЛИ 4-5 touchpoints без ответа → Перенести в nurture list, повтор через 3-6 месяцев
ЕСЛИ негативный ответ → Убрать из активного списка, не писать повторно
ЕСЛИ "Not interested" → "Понял, спасибо за честность. Если что-то изменится — буду на связи." + nurture
ЕСЛИ блокировка → Удалить из всех списков, не возвращаться
```

---

## Метаданные

- **Секция:** 4 — Prompt Library
- **Язык:** Русский (GOS), примеры параметров — English (как в реальном использовании)
- **Source tagging:** [→ repo] = из проиндексированных документов репозитория, [→ web] = внешние источники, [→ inferred] = логический вывод из данных
- **Актуальность:** Wave 1 Part 2 (2026-05-14)
- **Зависимости:** Секции 1-3 (Profile, Content, Outreach)
# Секция 5 — Content Engine

> Framework для выбора формата, тона и угла подачи под текущую ситуацию. Не список идей — а decision system.

---

## 5.1 Format Decision Framework

### Иерархия форматов по алгоритмической отдаче

| Формат | Относительная производительность | Ключевой сигнал | Когда применять |
|--------|----------------------------------|-----------------|-----------------|
| PDF carousel (8–10 слайдов) | **Highest (1.9x)** | Dwell time, completion rate | >3 пункта, сравнение, пошаговый процесс, checklist [→ repo] |
| Short video (<90s) | **Highest** | Engagement rate, completion | Демонстрация процесса, эмоциональный момент [→ repo] |
| Text-only | **High (+30% vs single-image)** | Reach, semantic coherence | Личная история, разбор кейса, opinion piece [→ repo] |
| Poll | **High reach, low conversion** | Superficial engagement | Валидация гипотезы, сбор данных, provocation [→ repo] |
| Image + text | **Medium** | Visual proof | Конкретный результат, метрика, скриншот дашборда [→ repo] |
| Single-image | **Lower (−30% reach)** | Reach penalty | Только когда визуал — главная ценность [→ repo] |
| External link | **Lowest (−60% reach)** | Reach suppression | Zero-click summary в посте, ссылка — только в комментарий [→ repo] |

### Decision Tree: «если X → то Y»

```
Если тема содержит >3 сравниваемых позиции, пошаговый процесс или checklist
    → Carousel (PDF, 8–10 слайдов) [→ repo]

Если тема — личный опыт, кейс, трансформация или «как я дошёл до…»
    → Text-only (800–1000 слов) [→ repo]

Если цель — проверить гипотезу, собрать мнение аудитории или спровоцировать обсуждение
    → Poll [→ repo]

Если есть конкретный визуальный результат: скриншот метрики, дашборд, до/после
    → Image + text (изображение = доказательство, текст = контекст) [→ repo]

Если нужно поделиться внешним ресурсом (статья, инструмент, репозиторий)
    → Text-only с zero-click summary; ссылку — только в первом комментарии [→ inferred]

Если контент мотивационный / без конкретики
    → Не публиковать; алгоритм подавляет vague language [→ repo]
```

### Правило 4 часов

Если предыдущий пост опубликован <4 часов назад — не публиковать новый. Алгоритм штрафует overposting [→ repo].

---

### Prompt P7 — Format Decision Engine

```
Роль: Content Format Strategist для LinkedIn Growth Operating System.
Задача: Выбрать оптимальный формат поста на основе входных параметров.

Входные параметры:
- topic: {topic} — тема поста
- goal: {goal} — цель (education / engagement / conversion / validation / authority)
- data_complexity: {data_complexity} — сложность данных (simple / moderate / complex / visual_proof)
- audience_intent: {audience_intent} — намерение аудитории (learn / discuss / save / share)

Правила выбора:
1. Если data_complexity = complex И >3 пункта → carousel
2. Если data_complexity = visual_proof → image + text
3. Если goal = validation → poll
4. Если goal = education И audience_intent = learn → text-only (800–1000 слов)
5. Если goal = engagement И topic = personal_experience → text-only
6. Если goal = authority И data_complexity = moderate → carousel или text-only
7. External links — только в комментарий; в посте zero-click summary
8. Никогда single-image без визуального proof

Выходной формат:
- recommended_format: [carousel | text | poll | image_text | video]
- rationale: [2–3 предложения обоснования]
- slides_count: [если carousel: 8–10]
- word_count_target: [если text: 800–1000]
- link_placement: [post_body | first_comment | none]
```

---

## 5.2 Tone & Angle Framework

### Четыре тона с критериями применения

| Тон | Когда применять | Критерии входа | Что измерять |
|-----|-----------------|----------------|--------------|
| **Educational** | Цель — передать навык, объяснить концепцию, разобрать ошибку | Есть чёткий learning outcome; применимо сразу; структурировано (framework, checklist, step-by-step) | Saves, bookmarks, «спасибо» в комментариях |
| **Opinion** | Цель — спровоцировать дискуссию, заявить позицию, отреагировать на тренд | Есть спорная теза; рискованно но не токсично; призыв к комментарию | Comment count, reply depth, sentiment ratio |
| **Storytelling** | Цель — эмоциональная вовлечённость, social proof, демонстрация пути | Есть narrative arc (до/после); личный опыт; урок в конце | Dwell time, profile views, connection requests |
| **Curatorial** | Цель — экономия времени аудитории, авторитет через отбор | >5 источников/инструментов/ресурсов; краткое резюме каждого; личная оценка | Shares, saves, tag mentions |

### Decision Tree: «если X → то Y»

```
Если у тебя есть конкретный фреймворк, checklist или пошаговая инструкция
    → Educational

Если ты хочешь сказать «я считаю, что X — ошибка» или отреагировать на хайп
    → Opinion

Если у тебя есть личный кейс с цифрами до/после и уроком
    → Storytelling

Если ты собрал 5+ инструментов/статей/ресурсов по теме и можешь оценить каждый
    → Curatorial

Если предыдущий пост был Educational и собрал >100 saves
    → Следующий: Storytelling (конвертировать внимание в доверие) [→ inferred]

Если предыдущий пост был Opinion и собрал >50 комментариев
    → Следующий: Educational (закрепить авторитет фактами) [→ inferred]

Если в профиле <500 connections
    → Приоритет: Storytelling > Educational > Curatorial > Opinion [→ inferred]

Если в профиле >1000 connections и SSI >70
    → Можно Opinion чаще: 1 на 3 Educational [→ inferred]
```

### Правило контраста

Никогда два подряд поста одного тона. Алгоритм и аудитория адаптируются; контраст поддерживает вовлечённость [→ inferred].

---

### Prompt P8 — Tone & Angle Selector

```
Роль: Tone Strategist для LinkedIn Growth Operating System.
Задача: Выбрать тон и угол подачи для поста на основе контекста.

Входные параметры:
- topic: {topic} — тема поста
- current_mood: {current_mood} — текущий настрой (confident / cautious / excited / reflective)
- previous_post_tone: {previous_post_tone} — тон предыдущего поста (educational / opinion / storytelling / curatorial / none)
- engagement_goal: {engagement_goal} — целевое действие (save / comment / share / profile_visit)
- connection_count: {connection_count} — текущее число коннекшнов
- ssi_score: {ssi_score} — текущий SSI (если известен)

Правила выбора:
1. Если previous_post_tone = educational И engagement_goal = profile_visit → storytelling
2. Если previous_post_tone = opinion → educational (контраст)
3. Если connection_count < 500 → storytelling или educational (avoid opinion)
4. Если ssi_score > 70 И topic = controversial → opinion
5. Если topic содержит список ресурсов/инструментов → curatorial
6. Если engagement_goal = comment → opinion или storytelling с provocation
7. Если engagement_goal = save → educational или curatorial
8. Если current_mood = reflective → storytelling
9. Если current_mood = confident И topic = trend_reaction → opinion

Выходной формат:
- recommended_tone: [educational | opinion | storytelling | curatorial]
- angle: [конкретный угол подачи в 1 предложении]
- opening_hook_type: [contrarian / question / number / story / challenge]
- risk_level: [safe / moderate — если opinion на острую тему]
- contrast_note: [почему этот тон контрастирует с предыдущим]
```

---

## 5.3 Category Mix Framework

### Правила ротации (не проценты)

```
1. После technical deep-dive (Educational, >800 слов, >50 saves)
    → Следующий пост: Storytelling (конвертировать внимание в доверие) [→ inferred]

2. После Opinion с высокой вовлечённостью (>50 комментариев)
    → Следующий пост: Educational (закрепить авторитет) [→ inferred]

3. Пропорция: 1 Opinion на 3 Educational [→ inferred]
    → Если за последние 3 поста не было Opinion — следующий может быть Opinion
    → Если последний был Opinion — следующий строго Educational или Storytelling

4. Poll — каждый 5-й пост [→ inferred]
    → Не чаще: superficial engagement не конвертирует в authority
    → Не реже: poll даёт reach boost и данные для следующих постов

5. Storytelling — минимум 1 из 4 постов [→ inferred]
    → Особенно важно при <500 connections: люди коннектятся с людьми, не с фреймворками

6. Curatorial — максимум 1 из 6 постов [→ inferred]
    → Высокая ценность, но риск «я просто собрал ссылки» без личного вклада

7. Никогда 2 подряд одного тона [→ inferred]
    → Educational → Storytelling → Educational → Opinion → Educational → Poll → ...

8. После viral post ( >10,000 impressions )
    → Следующий: Educational (capitalize на внимании с фактами) [→ inferred]
    → Не Opinion: риск потерить новую аудиторию резкостью
```

### Правило «вчерашнего комментария»

Если вчерашний комментарий к чужому посту собрал >10 likes — тема комментария → следующий пост (текущая тема горяча) [→ inferred].

---

### Prompt P9 — Content Mix Rotator

```
Роль: Content Mix Strategist для LinkedIn Growth Operating System.
Задача: Определить оптимальный тон и формат следующего поста на основе истории.

Входные параметры:
- last_5_posts: {last_5_posts} — список последних 5 постов в формате:
  [{tone, format, engagement_saves, engagement_comments, engagement_impressions}, ...]
- current_week_goal: {current_week_goal} — цель недели (authority / reach / engagement / connections)
- upcoming_events: {upcoming_events} — предстоящие события (conference / product_launch / industry_news / none)
- yesterday_top_comment: {yesterday_top_comment} — тема вчерашнего комментария с >10 likes (или none)
- current_connections: {current_connections} — текущее число коннекшнов

Правила ротации:
1. Если в last_5_posts >2 educational подряд → следующий: storytelling или opinion
2. Если в last_5_posts не было storytelling → следующий: storytelling
3. Если current_connections < 500 → storytelling или educational
4. Если upcoming_events ≠ none И событие через <7 дней → opinion или curatorial
5. Если yesterday_top_comment ≠ none → тема = yesterday_top_comment, тон = educational или opinion
6. Если current_week_goal = authority → educational или curatorial
7. Если current_week_goal = engagement → opinion или storytelling
8. Если current_week_goal = reach → poll или carousel
9. Poll допустим только если последний poll был >4 постов назад
10. Curatorial допустим только если последний curatorial был >5 постов назад

Выходной формат:
- next_post_recommendation:
    tone: [educational | opinion | storytelling | curatorial]
    format: [carousel | text | poll | image_text]
    topic_source: [yesterday_comment | upcoming_event | evergreen | audience_request]
    rationale: [2–3 предложения]
- sequence_warning: [если нарушено правило ротации — предупреждение]
- suggested_topic_angle: [1 предложение с углом темы]
```

---

## 5.4 Post Structure Framework

### Структура: Hook → Body → CTA

#### Hook (первые 50–70 слов)

- **44.2% LLM citations** приходятся на первые 30% текста [→ repo]
- **Dwell time** — primary ranking signal; hook определяет, прочитают ли дальше [→ repo]
- **Критерии качества hook**: конкретика, числа, контраст, вопрос, личное признание [→ inferred]

**Типы hook'ов (если X → то Y):**

```
Если есть контраст между «до» и «после» → Number + Contrast
    Пример: «3 месяца назад я обрабатывал 2 TB данных за 6 часов. Сегодня — за 18 минут.»

Если хочешь спровоцировать → Contrarian Statement
    Пример: «Airflow — не лучший выбор для 90% data pipeline. Вот почему.»

Если личный провал → Vulnerable Open
    Пример: «Я сломал продакшн базу в пятницу в 18:00. И это лучшее, что со мной случилось.»

Если список/сравнение → Curiosity Gap
    Пример: «Я протестировал 7 инструментов orchestration. 5 из них — мусор. 1 — неожиданно хорош.»

Если вопрос к аудитории → Targeted Question
    Пример: «Data Engineers из Алматы: как вы решаете проблему медленных pipeline в озёрах?»
```

#### Body (800–1000 слов оптимально)

- **+26% performance** для постов 800–1000 слов [→ repo]
- **Semantic coherence > keyword density** [→ repo]
- **Vague language** («I help businesses grow») лимитирует distribution [→ repo]
- **LLMs оценивают качество контента** и reward: frameworks, checklists, specific guidance [→ repo]

**Правила body:**

```
1. Каждый абзац — 1 мысль. Максимум 4 строки на абзац (mobile-first) [→ inferred]
2. Числа в каждом абзаце: метрики, даты, проценты [→ inferred]
3. Никогда «я помогаю компаниям расти» → всегда «я сократил latency pipeline с 6 часов до 18 минут» [→ repo]
4. Framework > Narrative: если можно представить как checklist или matrix — делать так [→ repo]
5. Личное местоимение (я/мы/наш) в первом и последнем абзаце; факты — в середине [→ inferred]
6. Переносы строки (line breaks) каждые 2–3 предложения для readability [→ inferred]
```

#### CTA (Call to Action)

- **Comments count 2x as much as likes** [→ repo]
- **Saves > likes** как trust signal [→ repo]
- **Critical engagement window**: первые 60 минут после публикации [→ repo]

**Типы CTA (если X → то Y):**

```
Если цель — комментарии → Open question в конце
    «Какой из этих подходов используете вы? Расскажите в комментариях.»

Если цель — saves → Explicit save ask
    «Сохраните — пригодится, когда будете проектировать следующий pipeline.»

Если цель — shares → Tag invitation
    «Знаете коллегу, которому это будет полезно? Отметьте в комментарии.»

Если цель — profile views → Personal note
    «Если тема resonates — добавляйтесь в сеть, обсудим подробнее.»

Если educational post → One-sentence summary + question
    «Коротко: декомпозируйте задачу, оркестрируйте через DAG, мониторьте через alerts. Как у вас?»
```

---

### Prompt P10 — Full Post Generator

```
Роль: LinkedIn Post Architect для Growth Operating System.
Задача: Сгенерировать полную структуру поста на основе параметров.

Входные параметры:
- topic: {topic} — тема поста
- format: {format} — формат (carousel | text | poll | image_text)
- tone: {tone} — тон (educational | opinion | storytelling | curatorial)
- persona: {persona} — роль автора (Data Engineer, Алматы, опыт X лет, стек Y)
- target_audience: {target_audience} — целевая аудитория (junior DE / senior DE / hiring managers / founders)
- engagement_goal: {engagement_goal} — целевое действие (save / comment / share / profile_visit)
- word_count: {word_count} — целевой объём (по умолчанию 800–1000 для text)
- hook_type: {hook_type} — тип hook'а (contrarian / question / number / story / challenge)

Правила генерации:

1. Hook (50–70 слов):
   - Если hook_type = number → начать с конкретной цифры + контраст
   - Если hook_type = contrarian → начать с «X — не Y» или «Забудьте про Z»
   - Если hook_type = story → начать с временной метки + личного признания
   - Если hook_type = question → задать вопрос, на который хочется ответить
   - Если hook_type = challenge → бросить вызов common wisdom
   - Обязательно: конкретика, отсутствие vague language

2. Body:
   - Если tone = educational → framework, checklist или step-by-step
   - Если tone = opinion → тезис → контраргумент → рефутирующий аргумент → вывод
   - Если tone = storytelling → narrative arc: context → conflict → action → result → lesson
   - Если tone = curatorial → 5+ пунктов с кратким резюме и личной оценкой каждого
   - Абзацы максимум 4 строки (mobile)
   - Числа в каждом абзаце
   - Semantic coherence: терминология консистентна на протяжении всего поста

3. CTA:
   - Если engagement_goal = comment → open-ended question
   - Если engagement_goal = save → explicit save ask
   - Если engagement_goal = share → tag invitation или «перешлите коллеге»
   - Если engagement_goal = profile_visit → personal invitation to connect

4. Формат-специфика:
   - Если format = carousel → для каждого слайда: заголовок слайда, 1–2 предложения, визуальный элемент
   - Если format = poll → вопрос + 2–4 варианта ответа + контекст в тексте (100–150 слов)
   - Если format = image_text → описание изображения + 200–400 слов контекста
   - Если format = text → полный текст 800–1000 слов

5. Мета-данные для tracking:
   - predicted_format: {format}
   - predicted_tone: {tone}
   - predicted_hook_type: {hook_type}
   - primary_keywords: [3–5 ключевых терминов для semantic coherence]
   - target_engagement: {engagement_goal}
   - posting_window: [Tue–Thu, 08:00–10:00 местного времени]

Выходной формат:
- post_structure:
    hook: [текст hook'а]
    body: [текст body с разбивкой на абзацы]
    cta: [текст CTA]
- carousel_slides: [если carousel: массив слайдов]
- poll_options: [если poll: массив опций]
- image_description: [если image_text: описание изображения]
- metadata: [объект с tracking-данными]
- compliance_check: [SAFE / MODERATE — если opinion на острую тему]
```

---

## 5.5 Content Intelligence Loop

### Metadata Tracking

Каждый пост должен записывать:

```
post_id: уникальный идентификатор
published_at: дата-время публикации
format: carousel | text | poll | image_text | video
tone: educational | opinion | storytelling | curatorial
topic_category: data_engineering | career | tools | industry | personal
hook_type: contrarian | question | number | story | challenge
word_count: число слов
has_external_link: true | false
posting_day: Mon | Tue | Wed | Thu | Fri | Sat | Sun
posting_hour: час публикации

engagement_metrics (через 7 дней):
  impressions: число показов
  likes: число лайков
  comments: число комментариев
  shares: число репостов
  saves: число сохранений
  profile_views: просмотры профиля (если доступно)
  new_connections: новые коннекшны (если связано)

performance_indicators:
  comment_to_like_ratio: comments / likes
  save_rate: saves / impressions
  engagement_rate: (likes + comments + shares + saves) / impressions
  dwell_time_estimate: высокий / средний / низкий (из carousel completion / text length)
```

### Правила анализа (если X → то Y)

```
Если comment_to_like_ratio > 0.5 → тон работает; повторить похожий угол [→ inferred]
Если saves > 20 при <1000 impressions → высокая ценность; сделать продолжение [→ inferred]
Если impressions < 500 при >500 connections → проверить semantic coherence и hook [→ inferred]
Если impressions растут после 24 часов → viral distribution; мониторить 96 часов [→ repo]
Если carousel completion rate низкий → уменьшить число слайдов до 5–7 [→ inferred]
Если text post >1200 слов и engagement низкий → сократить до 800–1000 [→ repo]
Если пост с external link в body → −60% reach ожидаемо; в следующий раз — link in comments [→ repo]
Если posting_day = Fri–Sun и engagement низкий → перенести на Tue–Thu [→ repo]
Если два educational подряд и второй провалился → вставить storytelling между ними [→ inferred]
Если opinion вызвал negative sentiment → следующий пост: educational с доказательствами [→ inferred]
```

---

### Prompt P11 — Performance Analyzer

```
Роль: Content Intelligence Analyst для LinkedIn Growth Operating System.
Задача: Проанализировать производительность поста и дать рекомендации.

Входные параметры:
- post_metadata: {post_metadata} — объект с полями:
  {post_id, published_at, format, tone, topic_category, hook_type, word_count,
   has_external_link, posting_day, posting_hour}
- engagement_data: {engagement_data} — объект с полями:
  {impressions, likes, comments, shares, saves, profile_views, new_connections}
- comparison_baseline: {comparison_baseline} — средние показатели за последние 10 постов:
  {avg_impressions, avg_likes, avg_comments, avg_shares, avg_saves, avg_engagement_rate}
- previous_posts: {previous_posts} — список последних 5 постов с их tone и performance

Правила анализа:

1. Рассчитать производительность:
   - engagement_rate = (likes + comments*2 + shares*3 + saves*2) / impressions
     [comments 2x, saves > likes — из алгоритмических сигналов] [→ repo]
   - save_rate = saves / impressions
   - comment_to_like_ratio = comments / likes
   - vs_baseline: impressions / avg_impressions, engagement_rate / avg_engagement_rate

2. Интерпретация:
   - Если impressions > 1.5x baseline И comments > 2x baseline → «Viral signal» [→ repo]
   - Если saves > 1.5x baseline → «High value content» [→ inferred]
   - Если comments > 2x baseline И sentiment positive → «Engagement winner» [→ inferred]
   - Если impressions < 0.5x baseline → «Distribution failure» [→ inferred]
   - Если likes > baseline НО comments < baseline → «Passive consumption» [→ inferred]
   - Если carousel И completion rate низкий (inferred из saves/shares ratio) → «Format mismatch» [→ inferred]

3. Диагностика:
   - Если distribution failure + has_external_link = true → причина: external link penalty [→ repo]
   - Если distribution failure + word_count > 1200 → причина: overlength [→ repo]
   - Если distribution failure + tone = opinion + connections < 500 → причина: early opinion [→ inferred]
   - Если distribution failure + posting_day = Fri/Sat/Sun → причина: timing [→ repo]
   - Если passive consumption + tone = educational → рекомендация: добавить provocation или question [→ inferred]
   - Если format mismatch + format = carousel → рекомендация: уменьшить слайды или сделать text [→ inferred]

4. Рекомендации:
   - Что повторить: элементы, которые дали +20% к baseline
   - Что изменить: элементы, которые дали −20% к baseline
   - Следующий пост: tone, format, topic на основе successful pattern
   - Если viral signal → предупреждение: мониторить 96 часов, не публиковать новый пост 48 часов [→ repo]

Выходной формат:
- performance_score: [0–100]
- classification: [viral | high_value | engagement_winner | average | underperformer | distribution_failure]
- vs_baseline:
    impressions: [x.x]
    engagement_rate: [x.x]
    saves: [x.x]
- root_cause: [если underperformer или distribution_failure — диагноз]
- what_worked: [массив элементов, которые дали +20%]
- what_to_change: [массив элементов, которые дали −20%]
- next_post_recommendation:
    tone: [recommended]
    format: [recommended]
    topic_angle: [recommended]
    hook_type: [recommended]
- alerts: [массив предупреждений: viral_monitoring / overposting_risk / format_mismatch / timing_issue]
```

---

## Source Summary

| Claim | Source |
|-------|--------|
| PDF carousels perform 1.9x | [→ repo] |
| Text-only +30% vs single-image | [→ repo] |
| Single-image −30% reach | [→ repo] |
| External links −60% reach | [→ repo] |
| Comments count 2x as much as likes | [→ repo] |
| Saves > likes as trust signal | [→ repo] |
| Dwell time = primary ranking signal | [→ repo] |
| 44.2% LLM citations from first 30% text | [→ repo] |
| Post length 800–1000 words +26% | [→ repo] |
| 2x/week posting = 5x profile views | [→ repo] |
| Semantic coherence > keyword density | [→ repo] |
| Vague language limits distribution | [→ repo] |
| LLMs reward frameworks, checklists, specific guidance | [→ repo] |
| First 60 minutes = critical engagement window | [→ repo] |
| Tue–Thu mornings +30% engagement | [→ repo] |
| Overposting <4 hours → penalties | [→ repo] |
| Content Distribution Phases (30-60 min / 2-6h / 24-96h) | [→ repo] |
| Carousel >3 пунктов/сравнения | [→ repo] |
| Text = личная история | [→ repo] |
| Poll = validation | [→ repo] |
| Image+text = результат | [→ repo] |
| All decision trees, tone rules, mix rules, structure rules | [→ inferred] |

---

*Framework version: 1.0 | Generated: 2026-05-14 | Dependencies: Section 1–4 indexed*
# 6. Метрики и триггеры

> Система сигналов, которые меняют поведение GOS. Не milestones — а decision rules.

---

## 6.1 Core Metrics Dashboard

| Метрика | Частота измерения | Инструмент | Target range |
|---------|-------------------|------------|--------------|
| **SSI (Social Selling Index)** | Еженедельно (понедельник) | LinkedIn Sales Navigator / стандартный SSI dashboard | 40–70 → 70+ [→ repo] |
| **Connection count** | Ежедневно | LinkedIn My Network | По milestone ranges (6.3) |
| **Acceptance rate** | Ежедневно (rolling 7-day window) | Ручной трекер / Expandi / Taplio | 25–50% [→ repo] |
| **Post impressions** | После каждого поста | LinkedIn Analytics (авторский пост → View analytics) | Baseline + рост 10–20% week-over-week [→ inferred] |
| **Post engagement rate** | После каждого поста | (Reactions + Comments + Shares) / Impressions | 2–5%+ [→ inferred] |
| **Profile views / неделя** | Еженедельно | LinkedIn «Who viewed your profile» | Рост 15–25% month-over-month [→ inferred] |
| **Incoming recruiter messages** | Еженедельно | LinkedIn Inbox (фильтр по Recruiter / Hiring Manager) | 1–3+/неделя к Month 2+ [→ inferred] |
| **Interview requests** | По факту | LinkedIn Inbox + календарь | **Основной KPI** — target: 2–5+ к Month 3 [→ inferred] |

**Anti-patterns:**
- Не отслеживать vanity metrics (всего connections без acceptance rate) [→ inferred]
- Не мерить follower count отдельно — для Data Engineer relevance > volume [→ inferred]
- Не использовать сторонние scraper-tools для метрик — риск restriction [→ repo]

---

## 6.2 Trigger Framework: Decision Rules

### Green Flags — ускориться

| Триггер | Порог | Действие |
|---------|-------|----------|
| Acceptance rate | >50% | +5 connection requests/day к текущему volume [→ inferred] |
| Engagement rate на посте | >5% | Запланировать пост на похожую тему в течение 3 дней [→ inferred] |
| SSI прирост | +5 за неделю | +1 пост/неделю к текущему rhythm [→ inferred] |
| Recruiter messages | 3+ за неделю | Активировать Phase 3 (Conversion & Interview Prep) [→ inferred] |
| Profile views | +50% week-over-week | Увеличить commenting activity на 50% [→ inferred] |
| Incoming interview request | 1+ | Логировать в Interview Tracker; удвоить posting frequency на неделю [→ inferred] |

### Red Flags — замедлиться / pivot

| Триггер | Порог | Действие |
|---------|-------|----------|
| Acceptance rate | <20% | -50% daily requests + пересмотреть Prompt P1 (Target Profile Fit) [→ repo] |
| Profile views | 0 в течение 3 дней | Срочно опубликовать пост или 3+ комментария к целевой аудитории [→ inferred] |
| Warning / restriction от LinkedIn | Любой | Остановить всю automation на 48ч; перейти на 100% ручной mode [→ repo] |
| SSI падение | -5 за неделю | Audit всех 4 компонентов SSI (6.4); пересмотреть content strategy [→ inferred] |
| Engagement rate | <1% на 3 поста подряд | Переключить формат (PDF carousel → text-only, или наоборот) [→ repo] |
| Pending invites | >500 | Приостановить outgoing requests на 3–5 дней [→ repo] |

### Neutral Adjustments

| Триггер | Действие |
|---------|----------|
| Acceptance rate 20–35% | Статус quo; микро-A/B test note в connection message [→ inferred] |
| Engagement rate 2–5% | Продолжать текущую тему; тестировать hook variations [→ inferred] |
| SSI стабилен (±2) | Фокус на 1 слабом компоненте SSI (6.4) [→ inferred] |
| Profile views стабильны | Добавить 1 новый commenting thread в неделю [→ inferred] |

---

## 6.3 Milestone Ranges

| Период | Connections range | SSI range | Сигналы |
|--------|-------------------|-----------|---------|
| **Week 2** | 50–100 | 20–35 | Профиль оптимизирован; первые comments + posts [→ repo] |
| **Month 1** | 150–300 | 30–50 | Стабильный acceptance rate; 2+ поста/неделю; первые recruiter views [→ repo] |
| **Month 2** | 400–700 | 50–70 | SSI 50+ = лимиты 75–150/неделю; первые incoming messages; 1+ interview [→ repo] |
| **Month 3** | 800–1200+ | 65–80+ | Conversion funnel активен; 2–5+ interviews; распределение через network [→ inferred] |

**Decision rule по milestones:**
- Если нижняя граница range не достигнута → активировать Red Flag audit (6.2)
- Если верхняя граница превышена → активировать Green Flag acceleration (6.2)
- Если в середине range → Neutral adjustment, фокус на качестве связей [→ inferred]

---

## 6.4 SSI Optimization Loop

### Компоненты SSI

1. **Establish your professional brand** — полнота профиля, posting consistency, content relevance [→ repo]
2. **Find the right people** — quality of search, saved leads, connection relevance [→ repo]
3. **Engage with insights** — comments, shares, posts that spark conversation [→ repo]
4. **Build relationships** — acceptance rate, message response rate, nurturing depth [→ repo]

### Decision Rule: SSI Component Audit

```
если SSI < 40 → фокус на компоненте 1 (brand) + снизить volume до 50–75/неделю
если SSI 40–70 и component 2 (find people) < 15/25 → audit Target Profile Fit (P1)
если SSI 40–70 и component 3 (engage) < 15/25 → увеличить commenting на 100%
если SSI 40–70 и component 4 (relationships) < 15/25 → audit DM sequence (P8–P10)
если SSI 70+ и component 3 (engage) максимален → +1 пост/неделю, scale connections до 150–200/неделю [→ repo]
```

---

### Prompt P12: SSI Analyzer

**Цель:** Автоматический аудит 4 компонентов SSI с рекомендациями по приоритету.

**Input:**
```yaml
ssi_score: {current_ssi}          # например, 52
component_scores:                 # каждый 0–25
  brand: {brand_score}
  find_people: {find_score}
  engage: {engage_score}
  build_relationships: {rel_score}
weekly_connections: {number}
acceptance_rate: {percentage}
posts_last_30_days: {number}
comments_last_30_days: {number}
profile_completeness: {percentage} # LinkedIn profile strength
```

**Output:**
1. Ranking компонентов по приоритету улучшения (слабейший первым)
2. Конкретное действие на неделю для каждого компонента
3. Прогноз SSI через 30 дней при выполнении рекомендаций
4. Триггер для GOS (Green / Red / Neutral) с порогом

**Authority rules:**
- При SSI < 40: запрещено увеличивать connection volume выше 75/неделю [→ repo]
- При component 3 (engage) < 15: приоритет commenting над posting [→ inferred]
- При component 4 (relationships) < 15: запрещено отправлять новые connection requests без audit P1 [→ inferred]

**Example:**
```
Input: SSI 48, brand 18/25, find_people 14/25, engage 8/25, build_rel 8/25
Output:
  Priority 1: engage (8/25) → 5 comments/day к целевой аудитории, 40+ words
  Priority 2: build_rel (8/25) → audit P1 (target fit), персонализировать note
  Priority 3: find_people (14/25) → использовать Saved Leads в Sales Navigator
  Priority 4: brand (18/25) → +1 Featured section item
  Forecast: SSI 58–63 через 30 дней
  Trigger: Neutral → Green if engage >15/25 within 14 days
```

---

## 6.5 Weekly Review Protocol

**Каждый понедельник, 15 минут:**

1. **Собрать данные** (5 мин):
   - SSI из Sales Navigator
   - Connection count, acceptance rate (rolling 7-day)
   - Profile views (прошлая неделя)
   - Incoming messages (recruiter + прочие)
   - Interview requests (новые + статус существующих)

2. **Применить триггеры** (5 мин):
   - Проверить каждый Green / Red flag из 6.2
   - Зафиксировать triggered actions

3. **Принять решение** (5 мин):
   - Если 2+ Green flags → ускорить на неделю
   - Если 1+ Red flag → замедлить + audit соответствующего prompt
   - Если нет флагов → продолжить текущий rhythm

**Инструмент:** Простой Markdown-файл или Notion база — не усложнять. [→ inferred]

---

## Source Summary

| Claim | Source |
|-------|--------|
| SSI tiers (<40 / 40–70 / 70+) и лимиты | [→ repo] |
| Acceptance rate 25–50% для SSI 40–70+ | [→ repo] |
| Volume Tax (>50 weekly + <30% acceptance → throttle) | [→ repo] |
| Pending invites cap ~500–700 soft flags | [→ repo] |
| 23% ban rate при aggressive automation | [→ repo] |
| Day 7 / Day 30 / 500+ milestones | [→ repo] |
| PDF carousels 1.9x performance | [→ repo] |
| Content format hierarchy | [→ repo] |
| All decision rules, triggers, ranges not explicitly cited | [→ inferred] |

---

**Next:** [Section 7: Risk Management & Recovery Protocols] (если создан) или обновление Master GOS интеграцией 6.1–6.5.
# Секция 7 — Bootstrap: как запустить GOS за 48 часов

**Цель:** Ввести LinkedIn Growth Operating System (GOS) в эксплуатацию с нуля до первого рабочего цикла за 48 часов. Не checklist — а пошаговый запуск системы.

---

## Час 0–2: Profile Foundation

**Цель:** Профиль проходит алгоритмическую проверку на «завершённость» (completeness = baseline algorithmic trust) [→ repo].

### 1. Фото профиля
- **Требование:** чёткое, смотрящее в камеру, нейтральный или тёплый фон.
- **Формат:** JPG/PNG, 400×400 px минимум.
- **Инструмент:** Canva (бесплатно) → Background Remover + Resize [→ web].
- **Decision framework:** если нет профессионального фото → используйте телефон при дневном свете, Canva для ретуши.

### 2. Баннер (Background Photo)
- **Размер:** 1584×396 px.
- **Контент:** 3 слоя — визуальный (Data/Cloud тема), социальное доказательство (логотипы компаний/сертификаты), призыв к действию (CTA).
- **Шаблон:** Canva → «LinkedIn Banner» → выберите Data/Technology [→ web].
- **Decision framework:** если нет логотипов работодателей → используйте иконки стека (AWS, Spark, Kafka) + текстовый CTA.

### 3. Headline Generator Prompt
- **Не пишите вручную.** Используйте **Headline Generator Prompt**.
- **Формула:** `{Role}` + `{Specialization}` + `{Outcome}` + `{Geography}`.
- **Пример структуры:** «Data Engineer | Cloud-Native Pipelines | Scalable ETL & Streaming | Central Asia».
- **Decision framework:** если <3 просмотров профиля в день после обновления → перезапустите Headline Generator с другим акцентом (outcome → specialization).

### 4. Location & Industry
- **Location:** Almaty, Kazakhstan (LinkedIn распознаёт как регион EMEA — влияет на рекомендации контента).
- **Industry:** Information Technology & Services / Computer Software.
- **Contact Info:** добавьте email (для InMail) и Telegram (для Kazakhstan-аудитории, если релевантно) [→ inferred].

---

## Час 2–6: Profile Content

**Цель:** Каждый блок профиля работает как конверсионная воронка: просмотр → интерес → действие (connect/message).

### 5. About Generator Prompt
- **Структура:** Hook → Problem → Solution → Proof → CTA.
- **Длина:** 300–500 символов первый абзац (до «See more»).
- **Используйте About Generator Prompt**.
- **Decision framework:** если <5% просмотров профиля кликают «See more» → сократите первый абзац до 200 символов или усильте hook.

### 6. Experience (STAR-формат)
- **Формат:** Situation → Task → Action → Result (метрики обязательны).
- **Пример:** «Построил ETL-пайплайн на Airflow + Spark для ритейл-клиента → сокращение latency с 6h до 45min, экономия $12k/мес».
- **Decision framework:** если нет метрик → используйте оценки ("сокращение времени обработки на ~60%") [→ inferred].

### 7. Skills (Top 3)
- **Приоритет:** 1) Primary skill (Data Engineering), 2) Adjacent (Cloud Architecture), 3) Differentiator (DataOps/MLOps).
- **Эндорсменты:** попросите 3–5 коллег в течение недели.
- **Decision framework:** если skill не отображается в «Top Skills» через 48h → проверьте частотность в вакансиях LinkedIn (Skills Insights).

### 8. Featured Section
- **Контент:** 2–3 документа — кейс-стади, презентация, или ссылка на GitHub/портфолио.
- **Формат:** PDF (1.9x performance boost vs обычные посты) [→ repo].
- **Инструмент:** Canva → экспорт в PDF, загрузить в LinkedIn Featured [→ web].
- **Decision framework:** если нет портфолио → создайте 1-страничный PDF «3 проекта Data Engineer» с GitHub-ссылками.

---

## Час 6–8: Tool Configuration

**Цель:** Все инструменты GOS сконфигурированы, протестированы, и готовы к работе в режиме zero-budget.

### 9. Промпты P1–P12: копирование и параметризация
- **Хранилище:** Google Sheets или Notion (бесплатно) [→ web].
- **Структура:** Колонки — Prompt ID, Назначение, Input Variables, Output Format, Использование.
- **Действие:** Скопируйте все операционные промпты (P1–P12) и генераторы профиля из соответствующих секций GOS в свой шаблон.
- **Параметризация:** замените `{input}` на ваши реальные данные (индустрия, география, роль).

### 10. Алгоритмы S1–S3: тестирование
- **S1 — Boolean Search Generator:** протестируйте 3 поисковых строки, проверьте количество результатов (цель: 500–2000 профилей).
- **S2 — Engagement Scoring:** убедитесь, что скрипт/формула корректно считает веса (comments 2x likes, saves > likes) [→ repo].
- **S3 — Content Calendar:** проверьте генерацию на 2 недели вперёд.
- **Decision framework:** если S1 возвращает <100 результатов → расширьте Boolean strings (добавьте OR-клаузы); если >5000 → добавьте AND-фильтры (география, компания) [→ inferred].

### 11. Автоматизация: n8n self-hosted + Google Alerts
- **n8n self-hosted:** бесплатно, требует VPS (~$5/мес на Hetzner/DigitalOcean) или локального сервера [→ web].
- **Google Alerts:** бесплатно, настройте 5–7 alerts по ключевым темам (Data Engineering, Cloud, Kazakhstan tech).
- **Интеграция:** n8n забирает Alerts → фильтрует → постит в ваш Notion/Telegram/Slack.
- **Kazakhstan note:** Hetzner (Финляндия/Германия) — стабильный доступ, нет блокировок [→ inferred].

### 12. SSI Baseline
- **Измерение:** LinkedIn Social Selling Index (бесплатно, https://www.linkedin.com/sales/ssi).
- **Запишите текущее значение и 4 компонента:**
  - Establish your professional brand
  - Find the right people
  - Engage with insights
  - Build relationships
- **Триггеры для действий:**
  - **SSI < 40:** лимит 50–75 invites/неделю, acceptance 15–25% → фокус на комментарии и контент, не на invites [→ repo].
  - **SSI 40–70:** лимит 75–150 invites/неделю, acceptance 25–35% → стандартный режим [→ repo].
  - **SSI 70+:** лимит 150–200 invites/неделю, acceptance 35–50% → можно увеличивать volume [→ repo].
- **Decision framework:** если SSI < 30 после 48h → проверьте completeness профиля (фото, headline, summary, experience, skills, connections <50) [→ inferred].

---

## Час 8–24: First Cycle (Day 1 работы GOS)

**Цель:** Пройти полный цикл GOS за 16 часов и зафиксировать baseline метрики.

**Timeline (Almaty, UTC+5):**

| Время (ALMT) | Действие | Инструмент / Prompt |
|--------------|----------|---------------------|
| 09:00 | **S1 — Boolean Search:** сгенерировать 1 таргетный сегмент (50–100 профилей) | S1 |
| 09:30 | **P4 — Comment Template:** написать 3 комментария к постам целевой аудитории (40+ слов каждый) | P4 |
| 10:30 | **P10 — Full Post Generator:** подготовить 1 пост (text-only, 800–1000 слов) | P10 |
| 11:00 | **Публикация поста** (оптимальное время для EMEA: 10:00–12:00 ALMT) [→ inferred] |
| 12:00 | **S2 — Engagement Scoring:** проскорить 20 профилей из S1 | S2 |
| 12:30 | **P1 — Connection Request:** отправить 10 персонализированных invites | P1 |
| 13:00 | **P2 — Follow-up DM:** отправить 3 follow-up сообщения (только тем, кто принял invite ранее) | P2 |
| 14:00 | **Review:** зафиксировать метрики (просмотры поста, accepts, replies, profile views) | Google Sheets |
| 18:00 | **Второй раунд комментариев:** 3 комментария к постам лидеров мнений | P4 |
| 20:00 | **Анализ первого поста:** dwell time, comments, likes (через LinkedIn Analytics) |
| 21:00 | **Планирование Day 2:** генерация через S3 (Content Calendar) | S3 |

**Лимиты Day 1 (консервативный режим, SSI <40 или неизвестен):**
- Connection requests: ≤10 [→ inferred]
- Comments: 5–6 (40+ слов) [→ repo]
- Messages (DM): ≤3 [→ inferred]
- Posts: 1 [→ repo]

**Decision framework:**
- Если acceptance rate <20% после 10 invites → пересмотреть headline (Headline Generator) и фото профиля [→ inferred].
- Если пост получил <50 просмотров за 6h → проверить время публикации (сдвинуть на 08:00 или 13:00 ALMT) и формат (возможно, PDF carousel вместо text-only) [→ repo].
- Если S2 показывает низкий engagement score у таргетных профилей → расширить S1 (добавить смежные роли: Analytics Engineer, ML Engineer) [→ inferred].

---

## Час 24–48: Calibration

**Цель:** Проанализировать данные первого цикла, откалибровать систему, спланировать первый значимый пост.

### 13. SSI Analyzer
- **Повторное измерение SSI** (если LinkedIn позволяет — иногда обновляется раз в сутки).
- **Сравнение с baseline:** какие компоненты выросли (обычно «Establish brand» растёт первым после оптимизации профиля).
- **Action:** если SSI не вырос после 48h → вернуться к Profile Foundation, проверить completeness [→ inferred].

### 14. Content Intelligence
- **Анализ первого поста:**
  - Impressions vs profile views (ratio цель: 1:10, т.е. 10% просмотров профиля от просмотров поста) [→ inferred].
  - Comments / Likes ratio (цель: >0.5, т.к. comments весят 2x в алгоритме) [→ repo].
  - Dwell time (LinkedIn не показывает напрямую, но прокси: time between post и first comment).
- **Сравнение с форматами:**
  - Text-only: baseline [→ repo].
  - PDF carousel: +90% reach (планируется на Day 3–5) [→ repo].
  - External links: −45–60% reach (избегать) [→ repo].

### 15. Анализ алгоритмов
- **Проверка на Volume Tax:** если отправили >50 invites за 7 дней и acceptance <30% → LinkedIn throttle до ~30 invites/неделю [→ repo]. Day 1–2 вы ещё не достигнете порога, но зафиксируйте текущий темп.
- **Rolling 7-day window:** начните Google Sheet для отслеживания invites/day [→ repo].
- **Pending invites cap:** держите <500 (soft flag) и точно <1,500 (hard cap) [→ repo].

### 16. Тюнинг системы
- **Prompt refinement:** если P3 дал слабый пост → добавьте в input 2–3 примера постов, которые вам понравились (few-shot prompting) [→ inferred].
- **S1 refinement:** если результаты поиска нерелевантны → проверьте Boolean operators (AND/OR/NOT) и добавьте исключения (`NOT recruiter`, `NOT HR`) [→ inferred].
- **Engagement shift:** если комментарии дали больше profile views, чем invites → переключите фокус на commenting (40+ слов, 2x/день) вместо cold invites на 3–5 дней [→ repo].

### 17. Планирование первого поста-максимум
- **Цель:** Пост, который даст максимальный рост SSI и connections.
- **Формат:** PDF Carousel (1.9x performance) [→ repo].
- **Тема:** «3 ошибки в Data Engineering, которые я совершил и как их избежать» (личный опыт + actionable advice = high dwell time + comments).
- **Инструмент:** Canva → Carousel (10–15 слайдов) → PDF → LinkedIn Document Post [→ web].
- **Публикация:** Day 3, 10:00 ALMT (после 2 дня комментирования для разогрева аудитории) [→ inferred].

---

## Bootstrap Checklist

Пройдите все 13 пунктов. Отмечайте по мере выполнения.

- [ ] **1.** Фото профиля загружено (400×400+, чёткое, профессиональное)
- [ ] **2.** Баннер создан и загружен (1584×396, 3 слоя: визуал/доказательство/CTA)
- [ ] **3.** Headline сгенерирован через Headline Generator и опубликован
- [ ] **4.** Location = Almaty, Kazakhstan; Industry = IT/Services
- [ ] **5.** About написан через About Generator (300–500 символов до «See more»)
- [ ] **6.** Experience заполнена в STAR-формате с метриками
- [ ] **7.** Top 3 Skills выбраны и размещены (Data Engineering, Cloud, DataOps)
- [ ] **8.** Featured Section содержит 2–3 PDF/документа
- [ ] **9.** Промпты P1–P12 и генераторы профиля скопированы в Google Sheets/Notion и параметризованы
- [ ] **10.** Алгоритмы S1–S3 протестированы (S1: 500–2000 результатов; S2: корректные веса; S3: 2 недели контента)
- [ ] **11.** n8n self-hosted или Google Alerts настроены для мониторинга
- [ ] **12.** SSI baseline зафиксирован (все 4 компонента записаны)
- [ ] **13.** First Cycle выполнен (Day 1 timeline пройден, метрики записаны)

---

## Decision Frameworks (быстрые правила)

| Если… | То… | Источник |
|-------|-----|----------|
| S1 не даёт результатов (<100 профилей) | Проверить Boolean strings → добавить OR-клаузы, убрать избыточные AND | [→ inferred] |
| Acceptance rate <20% | Пересмотреть Headline Generator (headline) и фото профиля | [→ inferred] |
| SSI < 40 после 48h | Проверить completeness профиля; снизить volume invites, увеличить commenting | [→ repo] |
| Пост <50 просмотров за 6h | Сдвинуть время публикации; попробовать PDF carousel вместо text-only | [→ repo] |
| Pending invites >500 | Остановить invites до clean-up; отозвать старые (>30 дней без ответа) | [→ repo] |
| Comments/Likes ratio <0.5 | Усилить CTA в постах («Поделитесь вашим опытом…»); увеличить длину поста до 800–1000 слов | [→ repo] |
| Invites/week >50 и acceptance <30% | Снизить до ~30 invites/неделю (Volume Tax) | [→ repo] |
| Нет профессионального фото | Снять на телефон при дневном свете + Canva Background Remover | [→ web] |

---

## Source Legend

- `[→ repo]` — данные из исследовательского репозитория (docs/01–11, pipeline, parameters)
- `[→ web]` — внешние источники (Canva, n8n, LinkedIn docs, инструменты)
- `[→ inferred]` — логический вывод на основе repo + web, структурированный как actionable recommendation

---

**Next:** Секция 8 — Daily Operations: ритуалы, метрики и корректировки.
