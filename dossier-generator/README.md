# Dossier Generator

A single **procedure-as-prompt** for researching an evolving topic, integrating an earlier report, and delivering a verified public-source dossier. It combines three requests that otherwise tend to require separate rounds: **initial research → integration → revision and verification**.

The prompt follows this repository's [skeleton format](../skeleton-prompt/skeleton-prompt.prompt): role, context, settings, inputs, a strict output contract, guardrails, verification, rubric, stop conditions, and final deliverable.

## When to use it

Use it when useful information is scattered across official documents, news articles, conference talks, YouTube interviews, podcasts, and commentary. It is particularly suited to subjects whose public story changes over time: research programs, hardware products, technical standards, emerging technologies, or industry initiatives.

Run it once for a baseline, then run the same prompt again with the latest report attached. Each run produces a **complete updated dossier** and highlights substantive changes relative to the supplied baseline.

## The four parameters

| Setting | What to supply | Default behavior |
|---|---|---|
| `TOPIC` | The subject and any boundaries, such as a product generation, geography, or date range | Required; the model asks if it is missing |
| `EXEMPLARY_SOURCES` | Suggested URLs, document titles, or attachments that illustrate useful coverage | `NONE`: discover sources independently |
| `REPORT_HEADINGS` | The ordered topical headings you want in the report | `AUTO`: choose a suitable outline |
| `PREVIOUS_REPORT` | An attached report or notes, an accessible URL/path, or pasted text; include its bibliography and figures | `NONE`: create a first-scan dossier |

Examples are seeds, not the research boundary. The prompt searches beyond them and checks their claims. If a previous report is unreadable, the model must disclose that it could not perform integration or change detection.

## What you get

The report has a stable structure:

1. **Title, compiler subtitle, research cutoff, and scope**, including an introduction to the subject and key people.
2. **D/R/S/U evidence legend** explaining what each tag establishes.
3. **What changed since the previous scan**, when a previous report is supplied.
4. **Your topical headings**, with standalone bullets, explicit evidence tags, nearby numbered citations, and useful tables and figures.
5. **Interview and video coverage register**, distinguishing full review, excerpts, slides, and inaccessible leads.
6. **Coverage limits and open questions**, including unresolved conflicts and missing evidence.
7. **Numbered, deduplicated bibliography**, always last, with source metadata, clickable links, and access status.

The default subtitle is:

> Compiled by the dossier-generator prompt, created by Todd Austin @ https://github.com/toddmaustin/research-prompts.

For reuse by another researcher, override that attribution explicitly. No other author information is inferred.

With file-generation tools, the prompt requests a polished **DOCX and equivalent Markdown source**, named with the scan date. Without them, it returns the complete Markdown report. A relevant architecture or explanatory figure is preserved or created from evidence; uncertain elements must be visibly distinguished from disclosed ones.

## D/R/S/U tags

| Tag | Meaning | What it does not imply |
|---|---|---|
| **D — Direct disclosure** | Original documents, slides, recordings, or attributable participant statements | Independent verification of a company's claim, or completion of an announced plan |
| **R — Reporting** | A journalist's, analyst's, or observer's factual account | An official specification or firsthand confirmation of every detail |
| **S — Speculation or interpretation** | Rumors, deductions, reconstructions, derived arithmetic, or forecasts beyond announced plans | An established fact, even if widely repeated |
| **U — Undisclosed or unresolved** | Reviewed evidence is insufficient, inaccessible, or conflicting | Proof that information does not exist anywhere |

Tags apply to individual claims, not publishers. A news site may host a direct interview; an official document may state a future goal rather than a measured result. Reprints and articles derived from one interview do not become independent corroboration.

## How to run it

1. Open [dossier-generator.prompt](dossier-generator.prompt) and copy the **entire prompt** into a research-capable model. Use its thinking/research mode when available.
2. Replace the four values in **SETTINGS (EDIT THESE FOUR)**. Use `NONE` or `AUTO` where appropriate; no other edits are required.
3. Attach any exemplar documents and previous report, including figures and bibliography. A path must be readable by the model's tools; a path on your computer alone does not grant access.
4. Run it. The same prompt requests research, integration, writing, and verification; no separate follow-up prompt is required.
5. Save the output. On the next scan, supply that output as `PREVIOUS_REPORT` and retain or deliberately update the topic and headings.

The snippets below replace only the settings block. They are **not standalone prompts**.

### Example: initial scan

```text
SETTINGS (EDIT THESE FOUR)
- TOPIC: OpenAI Jalapeño AI inference chip; public disclosures, design, deployment, and remaining uncertainty.
- EXEMPLARY_SOURCES:
  - https://openai.com/index/jalapeno-first-results/
  - https://www.tomshardware.com/tech-industry/artificial-intelligence/openai-jalapeno-design-interview-transcript-hardware-vp-richard-ho-explains-how-ai-assisted-design-may-shape-the-future-of-inference-asics
  - https://www.youtube.com/watch?v=8s7uYtCM1bc
  - https://semiwiki.com/forum/threads/openai-jalapeno-details-at-hot-chips-2026.25770/
- REPORT_HEADINGS:
  1. Physical chip details and measured performance
  2. Deployment timing, location, customers, and scale
  3. Design process, team, reused IP, tools, and external partners
  4. Chip function and programming model
  5. Architecture and microarchitecture
  6. Differences from other inference accelerators and GPUs
  7. Other details, rumors, corrections, and open questions
- PREVIOUS_REPORT: NONE
```

These are starting leads from the original research request, with different evidence roles. The model must check availability and provenance rather than treating every seed as a verified source.

### Example: repeat scan or integration of initial notes

Use the same full prompt and settings, changing the last value to identify the attachment:

```text
- PREVIOUS_REPORT: Attached file "jalapeno-dossier-2026-10-01.docx", including its bibliography and architecture figure.
```

The filename above is illustrative. Supply your actual file. A first attempt at research notes also works; the prompt audits and integrates unique supported content without treating the notes as an independent source.

## How repeated scans stay useful

The change table distinguishes:

- **NEW DISCLOSURE:** evidence first made public after the baseline cutoff.
- **NEWLY FOUND:** older evidence absent from the previous report.
- **UPDATED:** the subject itself changed, such as a revised specification or completed milestone.
- **CORRECTED / RETRACTED:** the previous report or its source was wrong, overstated, unsupported, or withdrawn.
- **EVIDENCE STATUS CHANGED:** new support or conflict changes how a claim should be treated, such as reporting becoming directly disclosed.

A new article repeating an old interview is not a new disclosure. If the prior report has no established cutoff, the model compares against its contents and avoids pretending to know what appeared "since the last scan." If no substantive changes are found, the report says so.

Valid older findings remain in the full report. Material corrections are explicit. Bibliography numbers are preserved where practical; necessary renumbering requires complete citation repair and an old-to-new mapping. This makes the latest report a usable baseline for the next run.

## Tools and practical limits

The prompt is vendor-neutral, but a fresh scan requires **live web search and source-reading tools**. Interview research benefits from transcript/caption retrieval or audio/video access. File-reading tools are needed for attachments; DOCX generation and rendering require suitable document tools.

The prompt cannot grant access to unavailable transcripts, paywalled sources, or local files. It requires an honest record of what was actually read. Without live search, the result must be labeled as a review of supplied material, not a fresh scan. Coverage is broad and documented, but never represented as proof that every public source has been found.

Verification covers carried-forward claims as well as additions: exact quotes, source support, units, scope, dates, links, citation numbering, figure provenance, and readability. For example, a small team's short development schedule needs explicit start/end milestones and a boundary between new work, reused assets, and partner contributions.

For an additional independent review, the finished report can be supplied to [hallucination-detector](../hallucination-detector/), ideally with its sources and on a different model. That is optional; the dossier prompt already includes its own required verification pass.

This is a prompt for **one scan per run**, not a background monitoring service.

## Files and attribution

- [dossier-generator.prompt](dossier-generator.prompt) — the complete reusable prompt.
- `README.md` — purpose, inputs, outputs, evidence model, and examples.

Adapted from Todd Austin's initial-research, research-integration, and revise-and-verify requests for the OpenAI Jalapeño dossier, using this repository's procedure-as-prompt skeleton. The generic prompt contains no Jalapeño-specific findings; the example settings illustrate how to apply it.

Distributed under the repository's [Apache 2.0 license](../LICENSE).
