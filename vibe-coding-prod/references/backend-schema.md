# Stage 5: Backend Schema

The Backend Schema defines every data model, relationship, and index.
A bad schema is technically debt that compounds forever. Get it right now.

---

## Stage 5 Template

```
## ── STAGE 5: BACKEND SCHEMA ────────────────────────────

### 5.1 Schema Philosophy
**Database type**: [PostgreSQL / MongoDB / MySQL / SQLite / etc.]
**ORM / Query builder**: [Prisma / Drizzle / TypeORM / SQLAlchemy / etc.]
**Naming convention**: snake_case tables, snake_case columns
**ID strategy**: [UUID v4 / CUID2 / auto-increment] — use UUID for distributed systems
**Timestamp convention**: All tables have `created_at`, `updated_at`, soft-delete via `deleted_at`

### 5.2 Entity Relationship Overview
```
users ──< sessions
users ──< [resource_plural]
[resource] ──< [child_resource]
users >──< teams (via team_members)
```

### 5.3 Core Tables / Collections

#### Table: users
| Column           | Type          | Constraints                    | Notes                     |
|------------------|---------------|--------------------------------|---------------------------|
| id               | UUID          | PK, DEFAULT gen_random_uuid()  |                           |
| email            | VARCHAR(255)  | UNIQUE, NOT NULL               | Lowercase, normalized     |
| email_verified   | BOOLEAN       | DEFAULT false                  |                           |
| password_hash    | VARCHAR(255)  | NULLABLE                       | Null for OAuth-only users |
| name             | VARCHAR(100)  | NOT NULL                       |                           |
| avatar_url       | TEXT          | NULLABLE                       |                           |
| role             | ENUM          | DEFAULT 'user'                 | user / admin / moderator  |
| plan             | ENUM          | DEFAULT 'free'                 | free / pro / enterprise   |
| stripe_customer_id | VARCHAR(50) | UNIQUE, NULLABLE              |                           |
| last_login_at    | TIMESTAMPTZ   | NULLABLE                       |                           |
| created_at       | TIMESTAMPTZ   | DEFAULT NOW()                  |                           |
| updated_at       | TIMESTAMPTZ   | DEFAULT NOW()                  |                           |
| deleted_at       | TIMESTAMPTZ   | NULLABLE                       | Soft delete               |

Indexes:
- `idx_users_email` ON (email)
- `idx_users_deleted_at` ON (deleted_at) WHERE deleted_at IS NOT NULL

#### Table: sessions
| Column       | Type         | Constraints              | Notes                   |
|--------------|--------------|--------------------------|-------------------------|
| id           | UUID         | PK                       |                         |
| user_id      | UUID         | FK → users.id ON DELETE CASCADE |                |
| token_hash   | VARCHAR(255) | UNIQUE, NOT NULL         | Store hash, not token   |
| expires_at   | TIMESTAMPTZ  | NOT NULL                 |                         |
| ip_address   | INET         | NULLABLE                 |                         |
| user_agent   | TEXT         | NULLABLE                 |                         |
| created_at   | TIMESTAMPTZ  | DEFAULT NOW()            |                         |

Indexes:
- `idx_sessions_token_hash` ON (token_hash)
- `idx_sessions_user_id` ON (user_id)
- `idx_sessions_expires_at` ON (expires_at) — for cleanup jobs

#### Table: [primary_resource]
| Column       | Type         | Constraints              | Notes                    |
|--------------|--------------|--------------------------|--------------------------|
| id           | UUID         | PK                       |                          |
| user_id      | UUID         | FK → users.id            | Owner                    |
| title        | VARCHAR(255) | NOT NULL                 |                          |
| content      | TEXT         | NULLABLE                 |                          |
| status       | ENUM         | DEFAULT 'draft'          | draft / published / archived |
| metadata     | JSONB        | DEFAULT '{}'             | Flexible extra fields    |
| created_at   | TIMESTAMPTZ  | DEFAULT NOW()            |                          |
| updated_at   | TIMESTAMPTZ  | DEFAULT NOW()            |                          |
| deleted_at   | TIMESTAMPTZ  | NULLABLE                 | Soft delete              |

Indexes:
- `idx_[resource]_user_id` ON (user_id)
- `idx_[resource]_status` ON (status)
- `idx_[resource]_created_at` ON (created_at DESC)
- Full-text: `idx_[resource]_fts` USING GIN (to_tsvector('english', title || ' ' || COALESCE(content, '')))

#### Table: oauth_accounts
| Column        | Type         | Constraints               | Notes                  |
|---------------|--------------|---------------------------|------------------------|
| id            | UUID         | PK                        |                        |
| user_id       | UUID         | FK → users.id ON DELETE CASCADE |               |
| provider      | VARCHAR(50)  | NOT NULL                  | google / github / etc  |
| provider_id   | VARCHAR(255) | NOT NULL                  |                        |
| access_token  | TEXT         | NULLABLE                  | Encrypted at rest      |
| refresh_token | TEXT         | NULLABLE                  | Encrypted at rest      |
| expires_at    | TIMESTAMPTZ  | NULLABLE                  |                        |
| created_at    | TIMESTAMPTZ  | DEFAULT NOW()             |                        |

Indexes:
- UNIQUE ON (provider, provider_id)
- `idx_oauth_user_id` ON (user_id)

#### Table: audit_logs
| Column       | Type         | Constraints    | Notes                          |
|--------------|--------------|----------------|--------------------------------|
| id           | UUID         | PK             |                                |
| actor_id     | UUID         | NULLABLE       | Null for system events         |
| action       | VARCHAR(100) | NOT NULL       | e.g. 'resource.created'        |
| resource_type| VARCHAR(50)  | NULLABLE       |                                |
| resource_id  | UUID         | NULLABLE       |                                |
| diff         | JSONB        | NULLABLE       | Before/after for update events |
| ip_address   | INET         | NULLABLE       |                                |
| created_at   | TIMESTAMPTZ  | DEFAULT NOW()  |                                |

Indexes:
- `idx_audit_actor_id` ON (actor_id)
- `idx_audit_resource` ON (resource_type, resource_id)
- `idx_audit_created_at` ON (created_at DESC)

### 5.4 Enum Definitions
```sql
CREATE TYPE user_role AS ENUM ('user', 'admin', 'moderator');
CREATE TYPE user_plan AS ENUM ('free', 'pro', 'enterprise');
CREATE TYPE resource_status AS ENUM ('draft', 'published', 'archived');
```

### 5.5 Row-Level Security (if using Postgres + Supabase/direct)
```sql
-- Users can only read/write their own rows
ALTER TABLE [resource] ENABLE ROW LEVEL SECURITY;
CREATE POLICY "user_owns_resource" ON [resource]
  USING (user_id = auth.uid());
```

### 5.6 Database Migrations Strategy
- Tool: [Prisma Migrate / Flyway / Alembic / custom]
- Never edit applied migrations — always create new ones
- Migration naming: `YYYYMMDDHHMMSS_description`
- Separate DDL migrations from data migrations
- Test migrations on staging before production

### 5.7 Caching Strategy
| Data            | Cache Layer    | TTL    | Invalidation              |
|-----------------|----------------|--------|---------------------------|
| User profile    | Redis          | 5 min  | On profile update         |
| Resource list   | Redis          | 1 min  | On create/delete          |
| Static config   | In-memory      | App lifetime | On deploy           |
| Session         | Redis          | Match token expiry | On logout    |

### 5.8 Data Privacy & Retention
| Data Category    | Retention  | Deletion Method           |
|------------------|-----------|---------------------------|
| User account     | Until deletion requested | Soft delete + GDPR purge job |
| Session tokens   | Until expiry | Hard delete via cron    |
| Audit logs       | 90 days   | Automated archival        |
| OAuth tokens     | Until account delete | Encrypted + purge  |

## ── END STAGE 5 ────────────────────────────────────────
```

---

## Schema Writing Rules

- Always store token hashes, never raw tokens.
- OAuth access/refresh tokens must be encrypted at rest (AES-256).
- Use JSONB (not TEXT) for structured flexible data — enables indexing.
- Soft deletes (`deleted_at`) are required for any user-facing data.
- Indexes must be planned before launch — adding them to production tables under load is painful.
- Row-level security prevents entire classes of authorization bugs.
- Audit logs are a compliance and debugging superpower.
