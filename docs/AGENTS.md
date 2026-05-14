# DOCS KNOWLEDGE BASE

**Generated:** 2026-05-14

## OVERVIEW

11-section research report (44,868 words) split for navigability. Each section is self-contained but assumes prior sections read in order.

## STRUCTURE

| File | Words | Purpose |
|------|-------|---------|
| `01-executive-summary.md` | ~4,538 | Pareto analysis: what works vs. what doesn't |
| `02-tactic-taxonomy.md` | ~3,619 | All tactics classified by risk and automation level |
| `03-platform-mechanics.md` | ~3,489 | Algorithm, limits, detection signals |
| `04-tool-landscape.md` | ~3,534 | Automation tools: features, pricing, risks |
| `05-conversion-mechanics.md` | ~2,967 | DM sequences, messaging frameworks |
| `06-pipeline-architecture.md` | ~2,519 | End-to-end workflow (see also `../pipeline/architecture.md`) |
| `07-prompt-surface-map.md` | ~3,635 | LLM prompt injection points for content gen |
| `08-parameterization-schema.md` | ~1,539 | Parameter definitions (see also `../parameters/schema.md`) |
| `09-evidence-quality.md` | ~4,457 | Source index and quality ratings |
| `10-open-questions.md` | ~2,991 | Known gaps, requested research |
| `11-recommended-stack.md` | ~11,580 | Implementation roadmap and tool recommendations |

## WHERE TO LOOK

| Task | File |
|------|------|
| What are the safest high-ROI tactics? | `01-executive-summary.md` |
| Will this tactic get me banned? | `02-tactic-taxonomy.md` + `03-platform-mechanics.md` |
| How does the LinkedIn algorithm work? | `03-platform-mechanics.md` |
| Which automation tool should I use? | `04-tool-landscape.md` |
| What DM template should I send? | `05-conversion-mechanics.md` |
| How do I build a content pipeline? | `06-pipeline-architecture.md` |
| How do I use AI to write posts? | `07-prompt-surface-map.md` |
| How do I adapt this to my industry? | `08-parameterization-schema.md` |
| How reliable is this claim? | `09-evidence-quality.md` |
| What isn't known yet? | `10-open-questions.md` |
| What should I do first? | `11-recommended-stack.md` |

## CONVENTIONS

- **Frontmatter** on every section: title, date, word count
- **Mermaid diagrams** for flows and architectures
- **Tables** for comparisons (tools, tactics, parameters)
- **Citations inline** — never footnotes; always `[SOURCE: ...]` adjacent to the claim

## ANTI-PATTERNS

- Don't duplicate `pipeline/architecture.md` or `parameters/schema.md` — cross-reference instead
- Don't change section numbering — external links and `linkedin_plan.md` depend on it
- Don't add new sections without updating `README.md` table and word count total
