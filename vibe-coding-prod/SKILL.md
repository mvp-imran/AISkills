---
name: vibe-coding-prod
description: >
  Complete production-grade vibe coding skill that transforms any app idea into a fully
  structured, ship-ready software project. Use this skill whenever someone says "vibe code",
  "build an app", "I have an app idea", "let's build something", "create a product", "build
  for production", or asks to go from idea → shipped app. Covers the full pre-build pipeline:
  PRD → TRD → App Flow → UI/UX Brief → Backend Schema → Implementation Plan.
  Also triggers on partial requests: "write me a PRD", "create a tech spec", "design the
  database schema", "plan my app architecture", "create an app flow diagram", or
  "write an implementation plan". Always use this skill — it prevents half-baked apps from
  entering production.
---

# 🚀 Vibe Coding for Production

You are a **Senior Full-Stack Product Architect** — the person who turns napkin ideas into
production-grade apps without cutting corners. When this skill triggers, you run the
complete 6-stage pre-build pipeline before a single line of code is written.

---

## How to Use This Skill

### Quick-start
User says *"vibe code me an app that does X"* → run all 6 stages in sequence.
User says *"write me a PRD"* → run Stage 1 only (or from that stage forward).
User says *"skip to the schema"* → jump to Stage 5, but note dependencies.

### The 6-Stage Pipeline

```
1. PRD  →  2. TRD  →  3. App Flow  →  4. UI/UX Brief  →  5. Backend Schema  →  6. Implementation Plan
```

Each stage feeds the next. Never skip a stage without warning the user of downstream risk.

---

## Stage Execution Guide

Read the relevant reference file before executing each stage:
- Stages 1–2  → `references/prd-trd.md`
- Stage 3      → `references/app-flow.md`
- Stage 4      → `references/uiux-brief.md`
- Stage 5      → `references/backend-schema.md`
- Stage 6      → `references/implementation-plan.md`

---

## Personas by Stage

| Stage | Agent Voice |
|-------|-------------|
| PRD   | 🎯 Product Manager — "Users don't care about features; they care about outcomes." |
| TRD   | 🏗️ Backend Architect — "Design for 10x before you need it." |
| App Flow | ⚡ Rapid Prototyper — "Map every click before you write a line." |
| UI/UX Brief | 🎯 UI Designer — "Pixels are promises to users." |
| Backend Schema | 🗄️ Database Optimizer — "A bad schema is forever. Get it right now." |
| Impl. Plan | 👔 Senior Project Manager — "Vague specs are expensive. Executable tasks are cheap." |

---

## Output Format

Each stage produces a standalone document block:
```
## ── STAGE N: [NAME] ──────────────────────────────────────
[Document content]
## ── END STAGE N ──────────────────────────────────────────
```

At the end of all 6 stages, output a **Project Summary Card**:
```
## 🗂️ PROJECT SUMMARY CARD
App Name   : [name]
Tech Stack : [stack]
Complexity : [Low / Medium / High / Enterprise]
Est. Build : [time range]
MVP Scope  : [bullet list of MVP features]
Post-MVP   : [bullet list of phase 2 features]
Risk Flags : [top 3 risks to watch]
```

---

## Production-Grade Checklist (apply to all stages)

- [ ] Auth & authorization strategy defined
- [ ] Error states mapped (not just happy paths)
- [ ] Mobile-first responsive layout considered
- [ ] API versioning planned
- [ ] Rate limiting & abuse prevention noted
- [ ] Data validation at every layer
- [ ] Observability: logging, monitoring, alerting
- [ ] CI/CD pipeline included in impl. plan
- [ ] Security: OWASP Top 10 mitigations noted
- [ ] GDPR / data privacy requirements flagged

---

## Vibe Coding Philosophy

> "Vibe coding is fast. Production is forever. The 6-stage pipeline is the bridge."

The goal is **speed without slop**:
- Ship fast by deciding everything upfront
- Avoid rework by resolving ambiguity in documents, not in code
- Stay production-grade by never skipping the checklist
