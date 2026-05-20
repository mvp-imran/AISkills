# Stage 3: App Flow

App Flow is the complete map of every user journey through the product.
It lives between the PRD (what) and the UI/UX Brief (how it looks).

---

## Stage 3 Template

```
## ── STAGE 3: APP FLOW ──────────────────────────────────

### 3.1 User Roles & Entry Points
| Role         | Entry Point    | Primary Flow          |
|--------------|----------------|-----------------------|
| Guest        | Landing page   | Sign-up → Onboarding  |
| Auth User    | Dashboard      | Core loop             |
| Admin        | Admin panel    | Management flows      |

### 3.2 Core User Flows

#### Flow 1: Onboarding
```
Landing Page
  ├─ [CTA: Sign Up] → Registration Form
  │     ├─ Success → Email Verification → Onboarding Wizard → Dashboard
  │     └─ Error   → Inline field errors → Re-try
  └─ [CTA: Log In] → Login Form
        ├─ Success → Dashboard (or last visited page)
        ├─ Wrong pass → Error + "Forgot password?" link
        └─ Forgot pw → Email sent → Reset form → Success → Login
```

#### Flow 2: [Core Feature Name]
```
Dashboard
  └─ [Action button] → [Feature Screen]
        ├─ Fill form → [Validation]
        │     ├─ Valid   → Submit → Loading → Success State → Updated View
        │     └─ Invalid → Inline errors → User corrects → Re-submit
        └─ Cancel → Back to Dashboard
```

#### Flow 3: Settings & Account
```
Profile Icon → Settings
  ├─ Profile tab  → Edit fields → Save → Success toast
  ├─ Security tab → Change password / Enable 2FA
  ├─ Billing tab  → Manage subscription → Stripe portal
  └─ Danger zone  → Delete account → Confirm modal → Logout → Landing
```

### 3.3 Page / Screen Inventory
| Screen Name       | Route              | Auth? | Components Needed         |
|-------------------|--------------------|-------|---------------------------|
| Landing           | /                  | No    | Hero, Features, CTA, Nav  |
| Register          | /register          | No    | Form, OAuth buttons       |
| Login             | /login             | No    | Form, OAuth buttons       |
| Dashboard         | /dashboard         | Yes   | Stats, Action bar, List   |
| [Feature] Detail  | /[resource]/:id    | Yes   | Detail view, Edit, Delete |
| Settings          | /settings          | Yes   | Tabs, Forms               |
| Admin Panel       | /admin             | Admin | Tables, Filters           |
| 404               | *                  | No    | Error illustration, CTA   |

### 3.4 State Management Map
| State           | Scope       | Storage     | Invalidation Trigger  |
|-----------------|-------------|-------------|-----------------------|
| Auth token      | Global      | Secure cookie / localStorage | Logout / expiry |
| User profile    | Global      | Store / context | Profile update  |
| [Resource] list | Page        | Local state | Create / delete action |
| UI loading      | Component   | Local state | Request complete      |

### 3.5 Error & Edge Case Flows
| Scenario                  | What Happens                              |
|---------------------------|-------------------------------------------|
| Session expired           | Redirect to /login with "session expired" |
| API 500 error             | Toast: "Something went wrong. Try again." |
| Empty state (no data)     | Illustrated empty state + primary CTA     |
| Offline / network error   | Banner: "No internet connection"          |
| Unauthorized (403)        | Redirect to dashboard + error toast       |
| Rate limited (429)        | Toast: "Too many requests. Wait X seconds"|

### 3.6 Navigation Structure
```
Top Nav: [Logo] [Primary Links] [User Menu]
Sidebar (if applicable):
  ├─ Dashboard
  ├─ [Feature 1]
  ├─ [Feature 2]
  └─ Settings
Mobile: Bottom nav bar with 4-5 primary destinations
```

## ── END STAGE 3 ────────────────────────────────────────
```

---

## App Flow Writing Rules

- Every flow must include error paths — happy paths alone are not production-ready.
- Empty states are features, not afterthoughts. Define them explicitly.
- Every screen in the inventory needs a route and auth requirement.
- Navigation structure must work on both desktop and mobile.
- State management scope prevents prop-drilling nightmares and re-render bugs.
