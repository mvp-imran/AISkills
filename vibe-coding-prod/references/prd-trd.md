# Stage 1: PRD — Product Requirements Document
# Stage 2: TRD — Technical Requirements Document

---

## STAGE 1: PRD

### Template

```
## ── STAGE 1: PRD ────────────────────────────────────────

### 1.1 Product Overview
**Product Name**: [Name]
**Tagline**: [One sentence value prop]
**Problem Statement**: [The exact pain being solved]
**Solution**: [How this app solves it uniquely]

### 1.2 Target Users
| Persona | Description | Primary Goal | Pain Point |
|---------|-------------|-------------|------------|
| [Name]  | [Who]       | [What]      | [Why now]  |

### 1.3 Core Use Cases (Top 5)
1. As a [user], I want to [action] so that [outcome].
2. ...

### 1.4 MVP Feature Set
| Feature | Priority | Why MVP | Acceptance Criteria |
|---------|----------|---------|---------------------|
| [F1]    | P0       | [reason]| [testable criterion]|

### 1.5 Out of Scope (MVP)
- [Feature X] — deferred to Phase 2 because [reason]
- [Feature Y] — ...

### 1.6 Success Metrics
| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| DAU    | [N]    | Analytics event    |

### 1.7 Constraints & Assumptions
- Budget: [range or unknown]
- Timeline: [target ship date]
- Team: [solo / small team / agency]
- Key assumption: [list assumptions that could invalidate the product]

### 1.8 Risks
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|

## ── END STAGE 1 ──────────────────────────────────────────
```

### PRD Writing Rules
- Be ruthlessly specific. "Fast" is not a requirement. "< 200ms API response under 1k concurrent users" is.
- Every feature must have an acceptance criterion.
- Assumptions must be explicit — hidden assumptions become production bugs.
- Out of scope is as important as in scope.

---

## STAGE 2: TRD

### Template

```
## ── STAGE 2: TRD ────────────────────────────────────────

### 2.1 System Architecture
**Pattern**: [Monolith / Microservices / Serverless / Hybrid]
**Justification**: [Why this pattern for this team/scale]

Architecture Diagram (text):
```
[Client] → [CDN] → [API Gateway] → [App Server] → [DB]
                                 ↘ [Auth Service]
                                 ↘ [Queue/Worker]
```

### 2.2 Tech Stack Decision
| Layer      | Choice     | Version | Why |
|------------|------------|---------|-----|
| Frontend   | [framework]| [ver]   |     |
| Backend    | [lang/fw]  | [ver]   |     |
| Database   | [db]       | [ver]   |     |
| Auth       | [solution] |         |     |
| Hosting    | [provider] |         |     |
| CDN        | [choice]   |         |     |
| CI/CD      | [pipeline] |         |     |
| Monitoring | [tool]     |         |     |

### 2.3 API Design Principles
- REST / GraphQL / tRPC (choose + justify)
- Base URL: `/api/v1/`
- Auth: JWT / Session / OAuth2 tokens via `Authorization: Bearer`
- Rate Limiting: [N] req/min per user, [M] req/min per IP
- Error format:
```json
{ "error": { "code": "RESOURCE_NOT_FOUND", "message": "...", "field": "..." } }
```

### 2.4 Core API Endpoints (MVP)
| Method | Endpoint             | Auth? | Description           |
|--------|----------------------|-------|-----------------------|
| POST   | /auth/register       | No    | Register new user     |
| POST   | /auth/login          | No    | Login, returns JWT    |
| GET    | /[resource]          | Yes   | List resources        |
| POST   | /[resource]          | Yes   | Create resource       |
| GET    | /[resource]/:id      | Yes   | Get single resource   |
| PUT    | /[resource]/:id      | Yes   | Update resource       |
| DELETE | /[resource]/:id      | Yes   | Delete resource       |

### 2.5 Non-Functional Requirements
| NFR         | Requirement                      | Test Method     |
|-------------|----------------------------------|-----------------|
| Performance | API p95 < 300ms                  | Load test       |
| Uptime      | 99.5% monthly                    | Uptime monitor  |
| Security    | OWASP Top 10 mitigated           | Security audit  |
| Scalability | Handle 10x traffic spike         | Stress test     |
| Data        | Backups every 6h, 30-day retain  | Backup verify   |

### 2.6 Infrastructure & Environments
| Environment | Purpose    | Scale         | Notes          |
|-------------|------------|---------------|----------------|
| Local       | Dev        | 1 instance    | Docker Compose |
| Staging     | QA/Review  | Prod-mirror   | Auto-deploy PR |
| Production  | Live       | Auto-scale    | Blue/Green     |

### 2.7 Security Requirements
- [ ] All secrets in env vars / secrets manager (never in code)
- [ ] HTTPS everywhere; TLS 1.2+ only
- [ ] Input validation + sanitization on all user input
- [ ] Parameterized queries / ORM (no raw SQL with interpolation)
- [ ] Rate limiting on auth endpoints
- [ ] Audit log for sensitive operations
- [ ] Dependency scanning in CI (e.g. Snyk, Dependabot)

### 2.8 Third-Party Integrations
| Service     | Purpose       | SDK / API   | Cost Tier |
|-------------|---------------|-------------|-----------|
| [Stripe]    | Payments      | Stripe SDK  | [tier]    |
| [SendGrid]  | Email         | REST API    | [tier]    |

## ── END STAGE 2 ──────────────────────────────────────────
```

### TRD Writing Rules
- Stack choices must be justified against the team's skills and the app's scale.
- NFRs must be measurable and testable, not aspirational.
- Every 3rd-party integration is a dependency and a risk — list them all.
- Security requirements are non-negotiable for production.
