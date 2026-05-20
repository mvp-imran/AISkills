# Stage 6: Implementation Plan

The Implementation Plan turns all previous documents into a sequenced,
executable build roadmap with milestones, tasks, and done criteria.

---

## Stage 6 Template

```
## ── STAGE 6: IMPLEMENTATION PLAN ──────────────────────

### 6.1 Build Strategy
**Approach**: [Iterative MVP → Expand / Feature-by-feature / Vertical slice]
**Team size**: [Solo / 2 devs / 3-5 devs]
**Estimated total duration**: [X weeks / months]
**Release strategy**: [Private beta → Public beta → GA / Hard launch]

### 6.2 Phase Overview
| Phase  | Name            | Duration | Deliverable                          |
|--------|-----------------|----------|--------------------------------------|
| 0      | Foundation      | Week 1   | Repo, CI/CD, DB, Auth working        |
| 1      | MVP Core        | Week 2-4 | All P0 features shipped and tested   |
| 2      | Polish & Launch | Week 5-6 | Performance, UX polish, launch-ready |
| 3      | Post-Launch     | Ongoing  | Feedback-driven iteration            |

---

### 6.3 Phase 0: Foundation (Week 1)

#### Goal: Everything is boring but works perfectly.

**Milestone: Repo & Tooling**
- [ ] Initialize repo with monorepo or single-repo structure
- [ ] Set up ESLint + Prettier + Husky pre-commit hooks
- [ ] Configure TypeScript (if applicable)
- [ ] Set up environment variable management (.env.local, .env.example)
- [ ] Write README with setup instructions
- **Done when**: Any team member can clone and run locally in < 5 min

**Milestone: CI/CD Pipeline**
- [ ] GitHub Actions (or equivalent) configured
- [ ] Lint + type-check on every PR
- [ ] Test suite runs on every PR
- [ ] Auto-deploy to staging on merge to main
- [ ] Deploy to production via manual trigger or tag
- **Done when**: A push to main auto-deploys to staging end-to-end

**Milestone: Database & ORM**
- [ ] Database provisioned (local via Docker + hosted staging)
- [ ] ORM installed and configured
- [ ] Initial migration: users, sessions tables
- [ ] Seed script with test data
- **Done when**: Can run seed script and query data from app code

**Milestone: Authentication**
- [ ] Register endpoint (email + password)
- [ ] Email verification flow
- [ ] Login endpoint (returns JWT / session)
- [ ] Middleware for protected routes
- [ ] Logout endpoint
- [ ] Password reset (email token flow)
- [ ] OAuth provider (Google / GitHub) — optional for MVP
- **Done when**: Complete register → verify → login → access protected → logout cycle works

**Milestone: Base UI Shell**
- [ ] Design tokens implemented (colors, typography, spacing) as CSS vars
- [ ] Layout component (nav + sidebar + main area)
- [ ] Base component library: Button, Input, Card, Toast, Modal, Skeleton
- [ ] Theme / dark mode toggle (if in scope)
- [ ] Responsive breakpoints validated
- **Done when**: Can render login/register pages with all component states

---

### 6.4 Phase 1: MVP Core (Weeks 2–4)

Build each feature as a full vertical slice: DB migration → API → Frontend → Tests.

**Feature development order** (P0 first, no exceptions):
1. [Feature 1] — [reason it unblocks other features]
2. [Feature 2]
3. [Feature 3]

**Per-feature task template:**
```
Feature: [Name]
  Backend:
    - [ ] DB migration (schema changes)
    - [ ] Service layer (business logic)
    - [ ] API endpoints (CRUD)
    - [ ] Input validation
    - [ ] Unit tests for service layer
    - [ ] Integration tests for API

  Frontend:
    - [ ] API client / hooks
    - [ ] Page/screen component
    - [ ] Loading + error + empty states
    - [ ] Form validation (client-side)
    - [ ] Responsive layout (mobile + desktop)

  Done when: Acceptance criteria from PRD are met + tested by a human
```

**Phase 1 Exit Criteria:**
- [ ] All P0 features from PRD implemented
- [ ] All features have loading, error, and empty states
- [ ] Mobile layout validated on real device
- [ ] Zero console errors or TypeScript errors
- [ ] Core user flow works end-to-end on staging

---

### 6.5 Phase 2: Polish & Launch (Weeks 5–6)

#### Performance
- [ ] Lighthouse score: Performance ≥ 85, Accessibility ≥ 90, SEO ≥ 80
- [ ] Images: WebP format, lazy loading, explicit width/height
- [ ] Code splitting / lazy loading for route chunks
- [ ] API response caching implemented (Redis or edge cache)
- [ ] Database query analysis: no N+1 queries, indexes validated

#### Security Hardening
- [ ] OWASP Top 10 checklist completed
- [ ] All secrets in secrets manager (not .env in production)
- [ ] Security headers configured (CSP, HSTS, X-Frame-Options, etc.)
- [ ] Rate limiting on all auth endpoints
- [ ] Dependency audit (npm audit / Snyk)
- [ ] Penetration test or security review

#### Observability
- [ ] Error monitoring set up (Sentry or equivalent)
- [ ] Application performance monitoring (APM) configured
- [ ] Uptime monitoring alert (5-min check)
- [ ] Structured logging (JSON logs, not console.log)
- [ ] Dashboards for key business metrics

#### Launch Readiness
- [ ] Custom domain + TLS configured
- [ ] Database backups scheduled and tested (restore drill)
- [ ] Production environment variables set
- [ ] GDPR / Privacy Policy / Terms of Service pages live
- [ ] 404 and 500 error pages designed
- [ ] Rollback plan documented

#### Phase 2 Exit Criteria (Launch Gate)
- [ ] Staging environment = production environment (config parity)
- [ ] End-to-end test of complete user journey passes
- [ ] Load test: app handles 10x expected day-1 traffic
- [ ] Rollback tested: can roll back deployment in < 5 min
- [ ] On-call runbook written

---

### 6.6 Phase 3: Post-Launch Iteration

**Week 1 post-launch:**
- Monitor error rates, p95 latency, and signup funnel daily
- Fix any P0 bugs within 24h
- Gather qualitative feedback (user interviews, support tickets)

**Ongoing iteration cadence:**
| Cadence   | Activity                                      |
|-----------|-----------------------------------------------|
| Daily     | Error monitoring review, key metric check     |
| Weekly    | User feedback synthesis, priority backlog sort|
| Bi-weekly | Sprint planning from ranked backlog           |
| Monthly   | Metric review against PRD success criteria    |

---

### 6.7 Tech Debt & Non-Negotiables

**Never defer:**
- Security vulnerabilities (fix in current sprint)
- Data loss risks (fix immediately)
- Accessibility blockers (fix before launch)

**Track and schedule:**
- Test coverage below 70%
- API endpoints without input validation
- Duplicate code > 30 lines

---

### 6.8 Definition of Done (apply to every task)

A task is only done when ALL of the following are true:
- [ ] Code reviewed and approved by at least 1 other person (or self-reviewed after 24h for solo)
- [ ] Tests written and passing
- [ ] No TypeScript errors / lint errors
- [ ] Runs correctly on staging
- [ ] Acceptance criteria from PRD/TRD met
- [ ] Mobile layout checked (if frontend)

## ── END STAGE 6 ────────────────────────────────────────
```

---

## Implementation Plan Writing Rules

- Phase 0 is sacred. Skipping foundation work is the #1 cause of vibe-coded apps not surviving launch.
- Build features in vertical slices — never finish "all backend" before starting frontend. Ship full working features.
- Every milestone has explicit "Done when" criteria. Fuzzy milestones lead to scope creep.
- The launch gate checklist is non-negotiable for production. Every item has caused a real outage somewhere.
- Post-launch monitoring must be set up before launch, not after the first incident.
