# PROJECT KNOWLEDGE BASE

**Generated:** 2026-05-14
**Commit:** 056f0c5
**Branch:** main

## OVERVIEW

Evidence-based research on LinkedIn profile growth (0 → 2,000+ connections). 44k+ words across 11 sections, 140+ cited sources. Pure Markdown — no code, no build step. Designed to be both human-readable and machine-consumable (parameterized pipeline, schema-driven tactics).

## STRUCTURE

```
.
├── docs/               # 11 research sections + nav README
├── pipeline/           # Executable growth pipeline architecture
├── tools/              # Automation tool comparison matrix
├── parameters/         # Parameterization schema (machine-readable inputs)
├── sources/            # (reserved for external source artifacts)
├── .github/            # CONTRIBUTING.md, issue templates
├── README.md           # Entry point: problem → file mapping
├── linkedin_plan.md    # AI agent prompt (autonomous plan generation)
└── LICENSE
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Find a section by topic | `README.md` problem table | Maps 12 common questions to files |
| Read the full research | `docs/01-executive-summary.md` → `docs/11-recommended-stack.md` | Sequential, 11 sections |
| Understand the growth pipeline | `pipeline/architecture.md` | Mermaid flowchart, parameterized stages |
| Choose automation tools | `tools/comparison-matrix.md` | Risk-aware comparison |
| Configure pipeline inputs | `parameters/schema.md` | Base params + derived variables + validation rules |
| Generate an AI growth plan | `linkedin_plan.md` | Prompt template for autonomous agents |
| Contribute corrections | `.github/CONTRIBUTING.md` | Style guide + acceptance criteria |

## CONVENTIONS

**Citations**
- Format: `[SOURCE: URL | Date | Quality]`
- Quality levels: `HIGH` / `MEDIUM` / `LOW`
- Vendor claims: `[VENDOR CLAIM — may be biased]`
- Estimates: `[UNVERIFIED ESTIMATE]`

**Risk Labels**
- Applied to all gray-area tactics: `SAFE` / `MODERATE` / `HIGH` / `ToS-violating`

**Cross-References**
- Section links: `[Section N: Title](NN-file-name.md)`
- Internal anchors use kebab-case headers

**Frontmatter**
- Docs sections include YAML frontmatter with metadata (title, date, word count)

## ANTI-PATTERNS (THIS PROJECT)

- **No automation code** — Research only; no scraping scripts, bots, or browser automation code
- **No unverified claims** — Every assertion requires a source citation
- **No undisclosed vendor marketing** — Bias must be tagged explicitly
- **No ToS violation encouragement** — ToS-violating tactics are documented for awareness, not recommended
- **Don't break sequential section order** — Sections build on prior context

## UNIQUE STYLES

- **Parameterizable pipeline** — All tactics accept base inputs (industry, persona, geography, risk tolerance) and derive behavior
- **Evidence quality hierarchy** — Peer-reviewed > official docs > practitioner case study > forum anecdote
- **AI-agent consumption** — `linkedin_plan.md` is structured as a prompt with authority rules so LLMs can generate personalized plans without hallucinating tactics

## COMMANDS

None. This is a documentation repository. Edit Markdown directly.

## NOTES

- `linkedin_plan.md` is a **prompt template**, not documentation. It contains strict authority rules (repo > internet) for autonomous AI agents.
- `sources/` is currently empty — intended for downloaded PDFs or snapshots of cited sources.
- Word counts in `docs/README.md` are manually maintained; check before trusting.
