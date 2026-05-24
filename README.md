# design-md

> **Machine-readable design specifications in Markdown** — structured visual contracts between designers, developers, and AI agents.

<p align="center">
  <a href="https://github.com/hmzainjamil/design-md/stargazers"><img src="https://img.shields.io/github/stars/hmzainjamil/design-md?style=for-the-badge&labelColor=555&color=yellow" alt="Stars"></a>
  <a href="https://github.com/hmzainjamil/design-md/forks"><img src="https://img.shields.io/github/forks/hmzainjamil/design-md?style=for-the-badge&labelColor=555&color=blue" alt="Forks"></a>
  <a href="https://github.com/hmzainjamil/design-md/issues"><img src="https://img.shields.io/github/issues/hmzainjamil/design-md?style=for-the-badge&labelColor=555&color=red" alt="Issues"></a>
  <a href="https://github.com/hmzainjamil/design-md/pulls"><img src="https://img.shields.io/github/issues-pr/hmzainjamil/design-md?style=for-the-badge&labelColor=555&color=green" alt="PRs"></a>
  <a href="https://github.com/hmzainjamil/design-md/commits/main"><img src="https://img.shields.io/github/last-commit/hmzainjamil/design-md?style=for-the-badge&labelColor=555" alt="Last Commit"></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Markdown-000000?style=flat&labelColor=555&logo=markdown" alt="Markdown">
  <img src="https://img.shields.io/badge/Design_Tokens-F24E1E?style=flat&labelColor=555" alt="Tokens">
  <img src="https://img.shields.io/badge/AI_Readable-4285F4?style=flat&labelColor=555" alt="AI">
  <img src="https://img.shields.io/badge/CSS-1572B6?style=flat&labelColor=555&logo=css3" alt="CSS">
</p>

---

## Why This Exists

Figma files rot. Storybook drifts from prod. Design docs in Confluence are never updated. DESIGN.md is a plain Markdown file checked into the repo alongside the code it describes. It version-controls with git, reads as context for AI agents, and is the authoritative source of truth for visual decisions.

---

## At a Glance

| Dimension | Detail |
|---|---|
| **Format** | Plain Markdown — renders on GitHub, Notion, anywhere |
| **Location** | `DESIGN.md` in project root or `docs/DESIGN.md` |
| **Sections** | Color, Typography, Spacing, Components, States, Motion |
| **Token format** | CSS custom properties + JSON export |
| **AI integration** | Readable as Claude/GPT context — agents implement from spec |
| **Version control** | Tracked in git — review design changes in PRs |
| **Tooling** | No plugins required — any text editor works |
| **Audience** | Developers, designers, AI agents, QA |
| **Output** | Living spec that generates CSS, Tailwind config, Figma variables |
| **Automation** | CI can validate component implementations against DESIGN.md spec |
| **Naming** | BEM for components, semantic for tokens |
| **Diff-ability** | Text-based — `git diff DESIGN.md` shows every visual change |

---

## 🧠 CONCEPTS

| Concept | Description | Why It Matters |
|---|---|---|
| **DESIGN.md** | Single Markdown file spec for a product's visual system | One file, version-controlled, always in sync |
| **Design Token** | Named, semantic key/value pair for a visual property | Decouple value from usage — change once, update everywhere |
| **Semantic Color** | `--color-error` not `--color-red` | Intent survives theme changes |
| **Component Spec** | Anatomy, states, variants, spacing defined in prose + table | Agent or dev can implement without asking questions |
| **State Inventory** | Default, hover, active, focus, disabled, loading, error | Missing states cause inconsistent UX bugs |
| **Motion Language** | Duration, easing, which elements animate and when | Prevents "animated everything" chaos |
| **Decision Log** | Record of why design decisions were made | Future devs understand intent, not just rules |
| **Constraint System** | Explicit rules about what's NOT allowed | Prevents component sprawl |
| **Breakpoint Contract** | Named breakpoints with semantic meaning, not just px | Teams share language: "below tablet" is unambiguous |
| **Accessibility Spec** | WCAG level, contrast minimums, focus behavior | Baked in — not bolted on |
| **Figma Parity** | DESIGN.md tokens match Figma variable values | Code and design never drift |
| **Agent Context** | AI reads DESIGN.md before implementing components | Zero back-and-forth for visual questions |

### 🔥 Hot

| Pattern | Detail | Why |
|---|---|---|
| Token semantics layer | `--color-interactive` wraps `--color-blue-600` | Theme-swappable without touching components |
| Component state table | 8-state matrix per component | No undocumented hover/focus states |
| Decision log section | `## Why` per major design choice | Future-proof — prevents regressions |
| AI context block | `<!-- AI: ... -->` HTML comments with implementation notes | Agents generate correct code first try |

---

## ⚙️ HOW IT WORKS

```
Designer writes DESIGN.md
          ↓
Developer reads DESIGN.md → implements components
          ↓
AI agent reads DESIGN.md → generates CSS/TSX from spec
          ↓
CI validates computed styles match token values in DESIGN.md
          ↓
git diff DESIGN.md shows every visual change in PR review
```

---

## 🚀 INSTALL

No installation. Just create `DESIGN.md` in your project root.

```bash
# Download the template
curl -o DESIGN.md https://raw.githubusercontent.com/hmzainjamil/design-md/main/DESIGN.template.md

# Or copy the minimal starter
cat > DESIGN.md << 'EOF'
# Design System

## Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--color-bg` | `#ffffff` | Page background |
| `--color-text` | `#111111` | Body text |
| `--color-accent` | `#0070f3` | CTAs, links |

## Typography

| Scale | Value | Usage |
|---|---|---|
| `--text-base` | `clamp(1rem, 2vw, 1.125rem)` | Body copy |
| `--text-xl` | `clamp(1.25rem, 3vw, 1.75rem)` | Section headings |

## Components

### Button

States: default, hover, active, focus, disabled, loading

| State | Background | Text | Border |
|---|---|---|---|
| default | `--color-accent` | white | none |
| hover | darken 10% | white | none |
| focus | `--color-accent` | white | `2px --color-accent` outline |
| disabled | `--color-accent` at 40% opacity | white | none |
EOF
```

---

## 📟 USAGE

### Feed to AI agent

```
Read DESIGN.md, then implement a React Button component 
that exactly matches the spec in the Button section.
```

### Generate CSS from spec

```bash
# Extract token table to CSS
python3 - << 'EOF'
import re, sys

with open('DESIGN.md') as f:
    text = f.read()

# Parse markdown tables with | Token | Value | Usage |
lines = []
for match in re.finditer(r'\|\s*`(--[\w-]+)`\s*\|\s*`([^`]+)`\s*\|', text):
    lines.append(f"  {match.group(1)}: {match.group(2)};")

print(":root {")
for l in lines:
    print(l)
print("}")
EOF
```

### CI validation

```yaml
# .github/workflows/design-check.yml
name: Design Token Validation
on: [pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Extract tokens from DESIGN.md
        run: python3 scripts/extract-tokens.py > /tmp/expected.css
      - name: Build computed CSS
        run: npm run build:css
      - name: Diff tokens
        run: diff /tmp/expected.css dist/tokens.css
```

---

## ⚙️ CONFIGURATION

| Section | Required | Format | Notes |
|---|---|---|---|
| Color tokens | Yes | Markdown table | CSS var names, hex/oklch values |
| Typography | Yes | Markdown table | clamp() for fluid scales |
| Spacing | Yes | Markdown table | Base unit + scale |
| Components | Yes | Prose + state table | All states, all variants |
| Motion | Recommended | Prose + table | Duration, easing, trigger |
| Breakpoints | Recommended | Markdown table | Name, px value, semantic meaning |
| Decision log | Recommended | Prose under `## Why` | Rationale for non-obvious choices |
| Accessibility | Yes | Table or checklist | WCAG level, focus policy |
| Dark mode | Conditional | Token overrides | Separate `:root[data-theme=dark]` block |
| Icons | Optional | Table | Name, source, usage |
| Imagery | Optional | Prose | Aspect ratios, treatment, naming |

---

## 💡 TIPS AND TRICKS

### Writing Specs

| Tip | Detail | Source |
|---|---|---|
| Write states before components | State inventory reveals edge cases early | [HMZ](https://github.com/hmzainjamil) |
| Use exact CSS values in tables | `clamp(1rem, 2vw, 1.5rem)` not "fluid" | [HMZ](https://github.com/hmzainjamil) |
| Include anti-patterns section | What NOT to do is as important as what to do | [HMZ](https://github.com/hmzainjamil) |

### AI Integration

| Tip | Detail | Source |
|---|---|---|
| Add `<!-- AI: ... -->` comments | Give agents implementation hints in HTML comments | [HMZ](https://github.com/hmzainjamil) |
| Put DESIGN.md path in CLAUDE.md | `Read DESIGN.md before implementing any UI component` | [HMZ](https://github.com/hmzainjamil) |
| Version DESIGN.md alongside components | PR should include DESIGN.md change + component change | [HMZ](https://github.com/hmzainjamil) |

### Maintenance

| Tip | Detail | Source |
|---|---|---|
| Add a `## Last Updated` section | Shows staleness at a glance | [HMZ](https://github.com/hmzainjamil) |
| Lock tokens after launch | Mark locked tokens with ⛔ — change requires RFC | [HMZ](https://github.com/hmzainjamil) |
| Link from README.md | `See [DESIGN.md](./DESIGN.md) for visual spec` | [HMZ](https://github.com/hmzainjamil) |

### Workflow

| Tip | Detail | Source |
|---|---|---|
| DESIGN.md PR review = design review | Every visual change goes through PR with diff | [HMZ](https://github.com/hmzainjamil) |
| Export to Figma variables | Token Studio reads JSON from DESIGN.md export script | [HMZ](https://github.com/hmzainjamil) |
| Generate Storybook story from spec | Script reads component state table → generates story template | [HMZ](https://github.com/hmzainjamil) |

---

## 🔧 TROUBLESHOOTING

| Issue | Cause | Fix |
|---|---|---|
| DESIGN.md ignored by team | Not referenced in CLAUDE.md or README | Add `Read DESIGN.md before any UI work` to CLAUDE.md |
| Token values drift from Figma | Manual sync process | Automate with Token Studio + export script |
| Component spec too vague | No state table, no exact values | Add exact CSS values, add 8-state matrix |
| AI agent ignores spec | Context window full before DESIGN.md loaded | Put DESIGN.md reference early in CLAUDE.md |
| CI validation failing | Token extraction regex too strict | Update regex to match your token naming pattern |
| DESIGN.md too long to read | Over-documented, includes rationale for everything | Split into `DESIGN.md` (spec) + `DESIGN-DECISIONS.md` (rationale) |
| Conflicting decisions across sections | No single owner, written by committee | Assign single DRI per section, mark with `Owner:` |
| Outdated on launch | No maintenance ritual | Add `DESIGN.md review` to sprint planning checklist |

---

## 📊 ARCHITECTURE

```
project/
├── DESIGN.md           ← living spec (this file)
├── scripts/
│   ├── extract-tokens.py   ← DESIGN.md → CSS/JSON
│   └── validate-design.js  ← CI token checker
├── src/
│   └── styles/
│       └── tokens.css      ← generated from DESIGN.md
└── .github/
    └── workflows/
        └── design-check.yml ← validates tokens in CI
```

---

## 🗺️ ROADMAP

| Priority | Feature | Status |
|---|---|---|
| P0 | DESIGN.md template | ✅ Done |
| P0 | Token extraction script | ✅ Done |
| P1 | Figma variables sync | 🔄 In Progress |
| P1 | CI validation GitHub Action | 📅 Planned |
| P2 | Storybook story generator | 📅 Planned |
| P2 | VS Code extension for preview | 📅 Planned |
| P3 | DESIGN.md → Tailwind config generator | 📅 Planned |

---

## ☠️ STARTUPS / BUSINESSES

| Old Way | Replaced By | Disruption |
|---|---|---|
| Figma as source of truth | DESIGN.md in git — version-controlled | 🔥 High |
| Zeroheight / Supernova ($500/mo) | Plain Markdown + GitHub | 💀 Total |
| Design handoff meetings | Agents read DESIGN.md directly | 🔥 High |
| Storybook maintenance | DESIGN.md spec + generated stories | 🔥 High |
| Custom design system tooling | Text file + 2 scripts | 💀 Total |

---

## Full DESIGN.md Template

```markdown
# [Project] Design System
<!-- AI: Read this entire file before implementing any UI component -->

## Metadata
- Version: 1.0.0
- Last Updated: [date]
- Owner: [name]
- WCAG Target: AA (2.2)

## Color Tokens

### Base palette
| Token               | Value               | Dark mode value     |
|---------------------|---------------------|---------------------|
| `--color-bg`        | `#ffffff`           | `#0a0a0a`           |
| `--color-surface`   | `#f8f8f8`           | `#111111`           |
| `--color-border`    | `#e5e5e5`           | `#222222`           |
| `--color-text`      | `#111111`           | `#ededed`           |
| `--color-muted`     | `#666666`           | `#888888`           |
| `--color-accent`    | `#0070f3`           | `#60a5fa`           |
| `--color-error`     | `#e5484d`           | `#f87171`           |
| `--color-success`   | `#30a46c`           | `#4ade80`           |
| `--color-warning`   | `#f76b15`           | `#fb923c`           |

## Typography

| Token             | Value                               | Usage         |
|-------------------|-------------------------------------|---------------|
| `--font-sans`     | `'Inter', system-ui, sans-serif`    | Body, UI      |
| `--font-display`  | `'Fraunces', serif`                 | Hero, H1      |
| `--font-mono`     | `'DM Mono', monospace`              | Code          |
| `--text-sm`       | `clamp(0.875rem, 1.5vw, 1rem)`      | Small labels  |
| `--text-base`     | `clamp(1rem, 2vw, 1.125rem)`        | Body copy     |
| `--text-lg`       | `clamp(1.125rem, 2.5vw, 1.375rem)` | Lead text     |
| `--text-2xl`      | `clamp(1.5rem, 4vw, 2.25rem)`       | Section heads |
| `--text-5xl`      | `clamp(3rem, 8vw, 6rem)`            | Hero          |

## Components

### Button
<!-- AI: implement with CVA variants, always include aria-busy on loading state -->

Variants: primary, secondary, ghost, destructive
Sizes: sm (32px), md (40px), lg (48px)

| State    | primary bg      | primary text | border  |
|----------|-----------------|--------------|---------|
| default  | `--color-accent` | white       | none    |
| hover    | accent -10% L   | white       | none    |
| focus    | accent          | white       | 2px outline offset 2 |
| disabled | accent 40% opacity | white    | none    |
| loading  | accent          | white       | none + spinner |
```

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/design-md&type=Date)](https://star-history.com/#hmzainjamil/design-md&Date)

---

Built by [HMZ](https://github.com/hmzainjamil)

---

## Resources

- [Design Tokens Community Group](https://www.designtokens.org)
- [Token Studio for Figma](https://tokens.studio)
- [Style Dictionary](https://amzn.github.io/style-dictionary)
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [CSS Custom Properties — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [Conventional Commits](https://www.conventionalcommits.org)
- [shadcn/ui](https://ui.shadcn.com)
- [CVA — Class Variance Authority](https://cva.style)

---

## Example: Component Decision Log

```markdown
## Why — Design Decisions

### Why no border-radius on buttons?
Decision: 0px radius on all interactive elements.
Reason: Swiss archetype — geometric clarity over softness. Radius implies friendliness; this product targets professional B2B users.
Date: 2024-01-15
Owner: HMZ

### Why Inter not Geist?
Decision: Inter for all body text.
Reason: Better letter spacing at 14px — Geist optimized for code/IDE environments, not dense body copy.
Date: 2024-01-10
Owner: HMZ

### Why oklch() not hex?
Decision: oklch() for all color tokens.
Reason: Better perceptual uniformity. Lightness scale 0.1→0.9 is visually consistent across all hues. Hex doesn't guarantee this.
Date: 2024-02-01
Owner: HMZ
```

---

## Example: Accessibility Spec

```markdown
## Accessibility

### Requirements
- WCAG 2.2 Level AA for all public-facing pages
- Level AAA for key conversion flows (pricing, checkout)

### Focus management
- All interactive elements must have visible focus ring
- Focus ring: 2px solid `--color-accent`, offset 2px
- Tab order must follow visual reading order
- Skip-to-content link as first element in `<body>`

### Color contrast minimums
| Context | Minimum ratio |
|---|---|
| Body text | 4.5:1 |
| Large text (>24px or >18.67px bold) | 3:1 |
| UI components (borders, icons) | 3:1 |
| Decorative elements | No requirement |

### Component-level requirements
| Component | Requirement |
|---|---|
| Button | `aria-disabled` when disabled, not `disabled` attr alone |
| Input | Always paired with `<label>` via `for/id` or `aria-labelledby` |
| Modal | Focus trapped inside, `aria-modal="true"`, Esc closes |
| Icon-only button | `aria-label` required |
| Loading state | `aria-busy="true"` on container |
| Error message | `aria-describedby` linking input to error |
```

---

## Motion Spec

```markdown
## Motion

### Principles
- Animate purpose, not decoration
- Respect `prefers-reduced-motion` — always
- Entry animations: fade + translate (never scale alone)
- Exit animations: fade only (no translate)

### Durations
| Interaction | Duration | Easing |
|---|---|---|
| Button press | 100ms | ease-in |
| Hover state | 200ms | ease-out |
| Panel open | 300ms | cubic-bezier(0.16, 1, 0.3, 1) |
| Page transition | 400ms | ease-in-out |
| Toast notification | 250ms in / 200ms out | ease-out / ease-in |

### Reduced motion override
All animations must wrap in:
```css
@media (prefers-reduced-motion: no-preference) {
  /* animation here */
}
```
Or use: `transition: none` when `prefers-reduced-motion: reduce`
```

---

## Spacing System

```markdown
## Spacing

Base unit: 4px

| Token | Value | Usage |
|---|---|---|
| `--space-1` | 4px | Icon padding, tight gaps |
| `--space-2` | 8px | Inner component padding |
| `--space-3` | 12px | Form field padding |
| `--space-4` | 16px | Standard padding |
| `--space-6` | 24px | Section gaps |
| `--space-8` | 32px | Card padding |
| `--space-12` | 48px | Large section gaps |
| `--space-16` | 64px | Page section padding |
| `--space-24` | 96px | Hero padding |

### Rules
- Never use px values directly in components — always use tokens
- Vertical rhythm: headings always have `margin-block-end: var(--space-4)`
- Stack spacing: siblings separated by `--space-6`
- Section spacing: `padding-block: var(--space-16)` minimum
```
