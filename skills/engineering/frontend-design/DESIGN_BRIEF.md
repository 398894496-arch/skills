# Design Brief: `frontend-design` Skill for Claude Code

> **Tagline:** Production-grade, opinionated UI design — without the AI aesthetic.

---

## What This Skill Does

`frontend-design` is a Claude Code skill that guides the end-to-end creation and refinement of frontend interfaces across React, Next.js, Tailwind CSS, and vanilla HTML/CSS/JS. It enforces a deliberate, named aesthetic direction on every project and runs a structured design-review pass after code generation to catch accessibility, hierarchy, and consistency issues before they ship.

---

## Scope

**Invoke this skill when:**

- Starting a UI from scratch (greenfield pages, components, landing pages, admin panels, dashboards)
- Re-theming or visually refactoring an existing React or Next.js page
- Running a design review or accessibility audit on existing markup
- Generating a design system foundation (tokens, typography scale, color palette)

**Do not invoke this skill for:**

- Pure logic or data-layer work (API routes, database schemas, state management)
- CLI tools, scripts, or non-visual output
- Documentation or prose writing
- Purely structural refactors that have no visible output

---

## Target Stacks

| Stack                | Notes                                                 |
| -------------------- | ----------------------------------------------------- |
| React (Vite / CRA)   | Component-first, CSS Modules or styled-components     |
| Next.js (App Router) | Server and client components, RSC-safe patterns       |
| Tailwind CSS         | Utility-first with design tokens in `tailwind.config` |
| Vanilla HTML/CSS/JS  | Semantic HTML5, custom properties, no frameworks      |

---

## Explicit Goals

1. **Opinionated aesthetics** — every project commits to a named visual direction (e.g., "editorial mono", "brutalist dashboard", "calm utility") before a single line of code is written.
2. **Strong UX and accessibility** — WCAG 2.2 AA compliance as a minimum bar, keyboard navigable, screen-reader friendly, reduced-motion support included by default.
3. **Responsive, production-ready markup** — fluid layouts, no magic pixel values, accessible at every breakpoint from 320 px to 1920 px.
4. **Design system thinking** — outputs use tokens (CSS custom properties or Tailwind config values) rather than hardcoded values, so the result scales and can be maintained.

---

## Explicit Anti-Goals (AI Slop Avoidance)

The following patterns are treated as design defects and must be avoided:

- **Colour** — default purple/blue gradients, absolute `#000000`/`#ffffff` without grounding rationale, gray text on a colored background, simultaneous competing accent colors.
- **Typography** — Inter or Roboto as default choices without a clear reason, mismatched font pairing (e.g., two geometric sans-serifs with identical optical weight), tight or airy line-height applied uniformly across all type sizes without a scale.
- **Layout** — generic SaaS hero (full-bleed gradient + centered headline + two CTA buttons + mockup screenshot), "cardocalypse" (every piece of content wrapped in a rounded card, including cards inside cards), modals used to hide complexity rather than reveal it.
- **Components** — oversized rounded Lucide/Heroicons above every heading, thick colored-border cards as a default container, emoji as primary iconography.
- **Effects** — glassmorphism for its own sake, neon or cyan-on-dark for "hacker aesthetic", elastic bounce animations on every interaction, sparklines and gradient accents to convey importance.
- **Copy** — redundant microcopy that restates the obvious ("Click the button to submit"), placeholder "Lorem ipsum" shipped in final output.
- **Accessibility** — low-contrast text, missing focus indicators, unlabeled interactive elements.

---

_This brief is the authoritative product spec for the skill. All prompt files and the SKILL.md must be consistent with it._
