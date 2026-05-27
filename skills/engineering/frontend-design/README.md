# /frontend-design

`/frontend-design` designs, refines, and audits production-grade frontend
interfaces across React, Next.js, Tailwind, and vanilla HTML/CSS/JS. It
enforces a named aesthetic direction before any code is written, encodes
anti–AI-slop constraints, and runs a WCAG 2.2-informed review after
generation.

The skill activates on requests like "design a UI", "build a component",
"re-theme this page", "run a design review", or "audit my frontend for
accessibility".

---

## Where this skill sits

Three entry modes, each with a matching prompt:

| Mode    | Prompt                     | Use when                   |
| ------- | -------------------------- | -------------------------- |
| Design  | `prompts/design.prompt.md` | Building a UI from scratch |
| Re-tune | `prompts/retune.prompt.md` | Refactoring an existing UI |
| Review  | `prompts/review.prompt.md` | Auditing existing markup   |

---

## What good output looks like

- A **named aesthetic direction** committed before any markup is written
  (e.g., "Tactile Dark Matter", "Precision Utility"), self-critiqued
  against the anti-slop and accessibility checklist.
- A **semantic token layer** (`--color-surface`, `--space-*`, named
  shadow scale, type scale) output first, then referenced throughout
  generated components — no hardcoded hex, no arbitrary spacing.
- Responsive markup at three breakpoints minimum, visible focus
  indicators, `prefers-reduced-motion` respected, WCAG 2.2 AA contrast.
- A **post-generation audit** report confirming the checklist passes
  and naming any fixes applied.

## What good output is not

- Full-bleed gradient hero with centered headline and two CTAs.
- Cardocalypse, glassmorphism as structure, default purple/indigo
  accents, Inter/Roboto with no type differentiation.
- Code that ships before a direction is committed.

---

## Files in this skill

| File                        | Role                                                                              |
| --------------------------- | --------------------------------------------------------------------------------- |
| `SKILL.md`                  | Orchestrator: 7-step iteration loop, hard rules, warning behaviour, review mode.  |
| `banned-patterns.md`        | Tiered (BLOCK / WARN / INFORM) anti-pattern catalogue with paired alternatives.   |
| `direction-doc-format.md`   | 9-field spec for `docs/design/direction.md` + bootstrap mechanics + ADR pairing.  |
| `design-tokens.md`          | Color / type / motion / spacing+depth token taxonomy used in Step 5.              |
| `prompts/design.prompt.md`  | Greenfield design flow.                                                           |
| `prompts/retune.prompt.md`  | Re-theme / refactor flow on existing components.                                  |
| `prompts/review.prompt.md`  | Design review / audit flow.                                                       |

---

## When this skill is the wrong tool

- Pure backend, data-layer, or API work.
- CLI tools, scripts, or non-visual output.
- Structural refactors with no visible change.
