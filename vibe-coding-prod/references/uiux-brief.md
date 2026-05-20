# Stage 4: UI/UX Brief

The UI/UX Brief is the single source of truth for how the app looks and feels.
It bridges App Flow (structure) and actual component building.

---

## Stage 4 Template

```
## ── STAGE 4: UI/UX BRIEF ──────────────────────────────

### 4.1 Design Philosophy
**Aesthetic**: [e.g. Clean & minimal / Bold & expressive / Enterprise utility]
**Primary Emotion**: [What should users feel? Trust / Excitement / Calm / Power]
**Design Inspiration**: [Reference products or styles, e.g. "Linear meets Notion"]
**Anti-patterns to Avoid**: [What this product should NOT feel like]

### 4.2 Brand Tokens
#### Colors
| Token               | Hex       | Usage                          |
|---------------------|-----------|--------------------------------|
| --color-primary     | #[hex]    | CTAs, active states, links     |
| --color-primary-hover | #[hex]  | Button hover                   |
| --color-secondary   | #[hex]    | Secondary actions              |
| --color-bg          | #[hex]    | Page background                |
| --color-surface     | #[hex]    | Card / panel background        |
| --color-border      | #[hex]    | Dividers, input borders        |
| --color-text-primary| #[hex]    | Headings, body copy            |
| --color-text-muted  | #[hex]    | Labels, placeholders           |
| --color-success     | #[hex]    | Success toasts, badges         |
| --color-warning     | #[hex]    | Warning states                 |
| --color-error       | #[hex]    | Error messages, validation     |

#### Typography
| Token              | Font        | Size  | Weight | Line-height | Usage       |
|--------------------|-------------|-------|--------|-------------|-------------|
| --text-display     | [font]      | 48px  | 700    | 1.1         | Hero H1     |
| --text-h1          | [font]      | 36px  | 700    | 1.2         | Page title  |
| --text-h2          | [font]      | 28px  | 600    | 1.3         | Section     |
| --text-h3          | [font]      | 22px  | 600    | 1.4         | Card title  |
| --text-body-lg     | [font]      | 18px  | 400    | 1.6         | Lead copy   |
| --text-body        | [font]      | 16px  | 400    | 1.6         | Body text   |
| --text-sm          | [font]      | 14px  | 400    | 1.5         | Labels      |
| --text-xs          | [font]      | 12px  | 500    | 1.4         | Badges, meta|

#### Spacing Scale (8pt grid)
`4 | 8 | 12 | 16 | 24 | 32 | 48 | 64 | 96 | 128`

#### Border Radius
| Token         | Value | Usage                  |
|---------------|-------|------------------------|
| --radius-sm   | 4px   | Badges, chips          |
| --radius-md   | 8px   | Cards, inputs          |
| --radius-lg   | 16px  | Modals, large panels   |
| --radius-full | 9999px| Pills, avatars         |

#### Shadows
| Token          | Value                           | Usage        |
|----------------|---------------------------------|--------------|
| --shadow-sm    | 0 1px 3px rgba(0,0,0,.1)        | Cards        |
| --shadow-md    | 0 4px 12px rgba(0,0,0,.12)      | Dropdowns    |
| --shadow-lg    | 0 8px 32px rgba(0,0,0,.16)      | Modals       |

### 4.3 Component Library
| Component     | Variants                        | States                        |
|---------------|---------------------------------|-------------------------------|
| Button        | Primary, Secondary, Ghost, Danger | Default, Hover, Active, Disabled, Loading |
| Input         | Text, Password, Search, Textarea | Default, Focus, Error, Disabled |
| Card          | Default, Hoverable, Selected    | Default, Hover, Loading       |
| Badge/Tag     | Default, Success, Warning, Error, Info | — |
| Toast/Alert   | Success, Warning, Error, Info   | With/without action button    |
| Modal         | Small, Medium, Full-screen      | With/without footer           |
| Avatar        | Image, Initials, Icon           | Online/Offline status         |
| Skeleton      | Text, Card, Avatar, Table row   | Shimmer animation             |
| Dropdown      | Select, Multi-select, Combobox  | Open, Closed, Disabled        |
| Nav           | Top bar, Sidebar, Bottom (mobile) | Active link, Collapsed      |

### 4.4 Layout System
| Breakpoint | Width        | Layout                 | Columns |
|------------|--------------|------------------------|---------|
| Mobile     | < 640px      | Single column          | 4       |
| Tablet     | 640–1024px   | 2-column               | 8       |
| Desktop    | 1024–1440px  | Multi-column + sidebar | 12      |
| Wide       | > 1440px     | Max-width 1280px, centered | 12  |

**Page Layout Structure:**
```
[Navbar 64px fixed]
[Sidebar 240px fixed (desktop)] | [Main Content area]
                                  [max-w-[1280px] mx-auto px-4/6/8]
```

### 4.5 Screen-by-Screen Wireframe Briefs

#### Landing Page
- Hero: Full-width, gradient BG, H1 + subheadline + primary CTA + product screenshot/mockup
- Social proof: Logo strip or testimonials
- Features: 3-col grid with icon + title + description
- CTA section: Centered, contrasting BG
- Footer: Links + social + legal

#### Dashboard
- Stat cards row (4 KPIs) with trend indicators
- Primary content area (list / table / chart)
- Quick-action floating button (mobile) or toolbar (desktop)
- Empty state if no data: illustration + primary CTA

#### [Core Feature Screen]
- Page header: Title + breadcrumb + action button
- Content: [describe layout specific to feature]
- Sidebar/panel for details (desktop) / bottom sheet (mobile)

#### Forms
- Single-column, max-width 480px, centered
- Label above input (not placeholder-as-label)
- Inline error messages below each invalid field
- Submit button: full-width on mobile, right-aligned on desktop
- Loading state on submit: spinner in button + disabled

### 4.6 Motion & Interaction Guidelines
| Interaction       | Animation             | Duration | Easing         |
|-------------------|-----------------------|----------|----------------|
| Page transition   | Fade in               | 150ms    | ease-out       |
| Modal open        | Scale 0.95→1 + fade   | 200ms    | ease-out       |
| Toast appear      | Slide in from top     | 250ms    | spring         |
| Button click      | Scale 0.97            | 100ms    | ease-in-out    |
| Skeleton → content| Cross-fade            | 300ms    | ease-in        |
| Hover (card)      | translateY(-2px)      | 150ms    | ease-out       |

### 4.7 Accessibility Requirements
- WCAG 2.1 AA minimum
- Color contrast: 4.5:1 for body text, 3:1 for large text
- All interactive elements keyboard-navigable (Tab, Enter, Escape)
- Focus rings visible (2px solid --color-primary)
- ARIA labels on icon-only buttons
- Form errors: `aria-describedby` linking field to error message
- Skip-to-content link as first focusable element

### 4.8 Dark Mode (if applicable)
- Use CSS custom properties for all colors (enables instant theme switching)
- Dark BG: #0f0f0f or #111827; Surface: #1a1a1a or #1f2937
- Shadows use rgba(0,0,0,0.5) (heavier in dark mode)
- Never use pure #000 or pure #fff

## ── END STAGE 4 ────────────────────────────────────────
```

---

## UI/UX Brief Writing Rules

- Use CSS custom properties (tokens) from day 1 — they make theming and dark mode free.
- 8pt grid is non-negotiable. Designers and devs align on the same system.
- Every component must have all interactive states specified — missing hover/disabled states become ugly bugs.
- Accessibility is a production requirement, not a nice-to-have.
- Wireframe briefs should be specific enough that a developer could build the layout without a Figma file.
