---
name: frontend-design
description: >
  Design and refine production-grade, opinionated frontend interfaces that
  avoid generic AI aesthetics. Use when asked to "design a UI", "build a
  component", "re-theme this page", "run a design review", or "audit my
  frontend for accessibility".
---

# Frontend Design Skill

Create, refine, and audit beautiful, intentional, production-ready frontend
interfaces across React, Next.js, Tailwind CSS, and vanilla HTML/CSS/JS.

---

## Overview

### What This Skill Does

This skill guides Claude Code through the complete frontend design lifecycle:

1. **Discovery** — gather brand, goals, audience, and stack constraints.
2. **Direction** — propose and commit to a named aesthetic direction before writing any code.
3. **Generation** — produce accessible, token-based, responsive markup and styles.
4. **Self-critique** — apply the built-in quality checklist immediately after generation.
5. **Audit / Review** — accept existing UI code and produce a structured compliance report plus an improved version.

### When to Use It

Invoke this skill when the request includes any of the following signals:

- "design", "build a UI", "create a component", "make a page"
- "re-theme", "restyle", "update the look and feel"
- "run a design review", "audit my UI", "check accessibility"
- "design system", "color palette", "typography scale"

### When Not to Use It

- Pure backend or data-layer work (API routes, DB queries, auth logic)
- Non-visual output (CLI tools, scripts, documentation)
- Structural refactors with no visible change

### Project Assumptions

- **Greenfield:** no prior design system exists; the skill builds tokens from scratch.
- **Existing codebase:** the skill reads current tokens and components before proposing changes; it does not silently overwrite a design system.
- For Next.js App Router projects, all generated components default to `"use client"` only when client-side interactivity is required; otherwise they are RSC-safe.

---

## Invocation & Inputs

### Auto-Invocation Triggers

Claude Code should activate this skill automatically when the user's request
matches one of the phrases in the `description` field above, or when a task
clearly involves creating or editing visual UI.

### Required Information

Before generating any code, collect the following. If any item is missing,
ask the user directly — do not invent values.

| Field                   | Question to Ask                                                 | Example                                           |
| ----------------------- | --------------------------------------------------------------- | ------------------------------------------------- |
| **Stack**               | "Which stack are you using?"                                    | Next.js 15 + Tailwind v4                          |
| **Intent**              | "What are you building, and who is it for?"                     | A SaaS analytics dashboard for data engineers     |
| **Brand**               | "Do you have brand colors, fonts, or a logo?"                   | Primary #1A1A2E, accent #E94560                   |
| **Content structure**   | "What are the key sections or components?"                      | Sidebar nav, data table, metric cards, chart area |
| **Constraints**         | "Any technical or design constraints?"                          | Must integrate with existing shadcn/ui tokens     |
| **Aesthetic direction** | "Any references or adjectives that describe the feel you want?" | Dense, editorial, print-inspired                  |

If the user says "I don't know" or "surprise me" for aesthetic direction,
proceed to the Direction Proposal step (see Iteration Loop).

---

## Behavioral Rules

### Typography

- Always pair fonts with a clear **role distinction**: one face for display/headings, one for body, optionally one for monospace data or code. Never use two faces of identical optical weight.
- Apply a **modular type scale** (e.g., Major Third or Perfect Fourth ratio). Never use a flat set of arbitrary font sizes.
- Set `line-height` relative to font size, not as a fixed pixel value. Body text: 1.5–1.65. Display headings: 1.05–1.2.
- Cap body line length at **60–75 characters** (use `max-ch` or equivalent). Never let prose columns span the full viewport width.
- **Avoid as defaults**: Inter, Roboto, Arial. These may only be used if the user's brand explicitly specifies them. Instead choose from less ubiquitous options (e.g., DM Sans, Syne, Epilogue, Fraunces, Space Grotesk, Instrument Serif, Manrope, Geist).
- Always declare fonts via CSS custom properties or Tailwind theme keys — never hardcode font-family strings in component JSX.

### Color Systems

- Build every palette from a **semantic token layer**: `--color-surface`, `--color-on-surface`, `--color-primary`, `--color-on-primary`, `--color-accent`, `--color-border`, `--color-muted`, plus state variants (error, warning, success, info).
- Derive light and dark theme values from the same token names; never use separate className switches for every dark-mode color.
- **Contrast minimums**: normal body text ≥ 4.5:1 against background (WCAG 2.2 AA); large text and UI components ≥ 3:1.
- **Avoid as defaults**: purple/indigo gradient pairs, `#000000`/`#ffffff` as literal values (use near-black and near-white with deliberate hue instead), simultaneous blue-and-purple accents.
- Do not use gray text on a colored background unless contrast is verified and intentional.
- Saturated colors on dark surfaces must be lightened to maintain contrast — never copy hex values from a brand guide without testing against the actual background.

### Layout

- Use a **named spacing scale**: define `--space-1` through `--space-10` (or Tailwind's spacing config) and reference only those tokens. Never write `margin: 13px`.
- Layouts must be defined at **three breakpoints minimum**: mobile (< 640 px), tablet (640–1024 px), desktop (> 1024 px). Use `clamp()` for fluid values where appropriate.
- Prefer **CSS Grid for page structure**, Flexbox for component-level alignment. Never use absolute positioning for primary layout.
- Respect safe areas on mobile (`env(safe-area-inset-*)` for fixed elements).
- Content containers should have a `max-width` that feels intentional, not a full-bleed default.
- Maintain **optical alignment** — visually align items to their perceptual edge, not their bounding box, especially for icons next to text.

### Motion and Depth

- Motion must be **purposeful**: use it to confirm actions, guide focus transitions, and reveal hierarchy — not as decoration.
- All animations must respect `prefers-reduced-motion`. Provide a `@media (prefers-reduced-motion: reduce)` block that removes or simplifies every transition.
- Duration: micro-interactions (< 150 ms), page transitions (200–400 ms), modal reveals (250–350 ms). Never use durations above 500 ms for UI feedback.
- Easing: use `ease-out` for elements entering the screen, `ease-in` for exiting, `ease-in-out` for position changes. Never use linear easing for visible motion.
- Elevation/shadow: define a **compact named scale** (3–5 levels: `shadow-sm`, `shadow-md`, `shadow-lg`, `shadow-xl`, `shadow-overlay`). Every component must use only named levels — no ad hoc `box-shadow` values.

### Imagery and Iconography

- Choose a **single icon family** per project and do not mix families (e.g., Lucide only, or Phosphor only).
- Icons used as standalone actions must have an `aria-label` or be accompanied by a visible label.
- Do not place an oversized icon (> 2rem) above a heading as the sole visual element — pair it with a meaningful illustration, photograph, or data visualization if visual weight is needed.
- Stock photography: if included, must have a direct connection to the content it accompanies. Never use vague "people at laptops" or "handshake" imagery as filler.
- Use `aspect-ratio` to prevent layout shift on image load. Always include `alt` text.

### Accessibility

- All interactive elements must be reachable by keyboard and have a **visible focus indicator** that meets WCAG 2.2 criterion 2.4.11 (focus not obscured) and 2.4.13 (minimum focus appearance).
- Every form input needs a programmatically associated `<label>`. Never use `placeholder` as a substitute for a label.
- Landmark regions (`<main>`, `<nav>`, `<aside>`, `<header>`, `<footer>`) must be present on every page-level layout.
- Color alone must never convey meaning (e.g., a red border for error must also include an icon or text).
- Images: non-decorative images require descriptive `alt`; decorative images use `alt=""`.
- ARIA: use native HTML semantics before reaching for ARIA roles. `role="button"` on a `<div>` is a defect.

---

## Anti–AI-Slop Constraints

These are hard rules, not suggestions. Violating them is treated as a defect.

### The Named Direction Rule

**Before writing any code**, the skill must:

1. State the aesthetic direction in one sentence (e.g., "This dashboard will follow a dense, editorial-print direction with a dark ink-paper palette and a condensed serif/tabular-mono type pair.").
2. Confirm with the user or proceed if the user already approved a direction.
3. Never start coding without a committed direction.

If the user later requests a change that contradicts the committed direction (e.g., "add a gradient glow to this editorial layout"), the skill must:

- Name the conflict explicitly: "That gradient glow would clash with the editorial-print direction we committed to."
- Propose a direction-consistent alternative.
- Only override if the user explicitly acknowledges and accepts the direction change.

### Banned Default Patterns

The following patterns may not appear in generated output unless the user has explicitly requested them and acknowledged the trade-off:

| Pattern                                                                         | Why It's Banned                                                              |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Full-bleed gradient hero (gradient + centered headline + two CTAs + screenshot) | Used in >60% of AI-generated SaaS pages; signals no creative investment      |
| Cardocalypse (cards within cards, every section wrapped in a rounded card)      | Produces visual noise and destroys hierarchy                                 |
| Purple/indigo as default accent                                                 | The statistically most common AI-generated UI accent; signals generic output |
| Glassmorphism as a structural element                                           | Legibility degrades in real content scenarios                                |
| Neon/cyan on dark for "hacker" or "tech" vibe                                   | Overused to the point of being a parody                                      |
| Oversized rounded icon above heading with no supporting content                 | Padding a layout without adding information                                  |
| Thick colored-border cards as default container                                 | Second most common AI card pattern after shadow-heavy cards                  |
| `Inter` or `Roboto` without explicit brand justification                        | Not wrong, just a signal that no type decision was made                      |
| Linear or elastic bounce on every button click                                  | Treats motion as noise, not signal                                           |
| Empty CTA copy ("Get started", "Learn more")                                    | Copy defect, not a design defect — but flag it                               |

### Warning Behavior

When the user requests a banned pattern, respond with:

```
⚠️ Design direction conflict: "[pattern]" is a common AI-generated UI signature
   that undermines the "[direction name]" aesthetic.

   Alternative that fits the current direction:
   → [specific alternative with brief rationale]

   If you want to override, say "proceed anyway" and I will implement it.
```

---

## Iteration Loop

### Step 1 — Direction Proposal

After gathering inputs, propose **2–3 named aesthetic directions**. For each:

- Give it a short name (e.g., "Tactile Brutalism", "Warm Editorial", "Precision Utility")
- Write one paragraph describing the visual feel
- List: typography approach, primary palette, layout personality, motion character
- Flag any risks or constraints (e.g., "Heavy serif may not render well at small sizes on Android")

### Step 2 — Self-Critique Each Direction

Apply the following checklist to each proposed direction before presenting it:

- [ ] Does this direction avoid all banned default patterns?
- [ ] Is the type pairing genuinely differentiated (distinct roles, different optical weight)?
- [ ] Can the color palette achieve WCAG 2.2 AA contrast in both light and dark modes?
- [ ] Does the layout approach work at 320 px without horizontal scrolling?
- [ ] Is the motion character purposeful rather than decorative?
- [ ] Is this direction consistent — would the 10th component still feel like the 1st?

Flag any direction that fails two or more checks. Offer a refined version instead.

### Step 3 — Direction Confirmation

Present the proposals. Wait for user feedback. If the user selects one,
confirm: "Proceeding with **[Direction Name]**. I'll stay consistent with
this throughout." Do not deviate without explicit instruction.

### Step 4 — Code Generation

Generate markup and styles following all Behavioral Rules. Annotate
significant design decisions with brief inline comments so the user
understands why each choice was made (not just what it is).

### Step 5 — Post-Generation Audit

Immediately after generating code, run the Design Review Checklist (see
Review Mode below) against the output. If any item fails, apply fixes and
note them in a short post-generation report:

```
✅ Post-generation audit:
   - Contrast: all text passes WCAG 2.2 AA ✓
   - Focus indicators: added to all interactive elements ✓
   - Spacing: normalized to token scale ✓
   - [Issue found]: fixed by [action taken]
```

---

## Review Mode (Design Audit)

### Trigger Phrases

Activate review mode when the user says:

- "review my UI", "audit this design", "check accessibility"
- "run a design review", "check this against web guidelines"
- "what's wrong with this UI"

### How It Works

1. Ask for the file(s) or pattern to review if not provided.
2. Fetch and apply the Vercel Web Interface Guidelines from:
   `https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`
   (Use WebFetch to retrieve live rules each time.)
3. Apply the internal checklist below.
4. Produce a structured report in `file:line` format.
5. Output a revised version of the code with all fixable issues resolved.

### Design Review Checklist

#### Accessibility

- [ ] All images have appropriate `alt` text
- [ ] All form inputs have associated `<label>` elements
- [ ] All interactive elements are keyboard-accessible
- [ ] Focus indicators are visible and meet WCAG 2.2 AA (2.4.11, 2.4.13)
- [ ] Color is not the only means of conveying meaning
- [ ] ARIA is used only where native semantics are insufficient
- [ ] `prefers-reduced-motion` is respected for all animations

#### Readability and Hierarchy

- [ ] Heading levels are sequential (no skipped levels)
- [ ] Body text meets minimum contrast (4.5:1 normal, 3:1 large/UI)
- [ ] Line length is capped at ~75 characters for prose
- [ ] Line height is appropriate per text size (not uniform across all sizes)
- [ ] Type scale is consistent and uses named tokens

#### Layout and Spacing

- [ ] All spacing values reference the design token scale
- [ ] Layout is tested at 320 px, 768 px, and 1280 px
- [ ] No content is hidden behind fixed headers without scroll-margin compensation
- [ ] Safe area insets applied for fixed/sticky elements on mobile

#### Color and Contrast

- [ ] Semantic color tokens are used (not hardcoded hex)
- [ ] Dark mode (if present) is derived from the same token names
- [ ] No gray text on colored background without verified contrast
- [ ] Elevation/shadow system uses only named scale levels

#### Microcopy and Content

- [ ] CTA labels describe the action (not "Click here" or "Submit")
- [ ] Error messages explain what went wrong and how to fix it
- [ ] Loading and empty states are defined for all dynamic content
- [ ] No placeholder copy ("Lorem ipsum") in generated output

### Review Report Format

```
## Design Review: [filename or component name]
Date: [date]
Aesthetic direction: [detected or stated]

### Critical (must fix before ship)
[file]:[line] — [rule violated] — [suggested fix]

### Recommended (should fix)
[file]:[line] — [rule] — [suggested improvement]

### Informational
[file]:[line] — [observation, no action required]

### Summary
[N] critical, [N] recommended, [N] informational issues found.
Revised code follows.
```

---

## Output Format

- Code: fenced blocks with language tag (e.g., ` ```tsx `, ` ```css `)
- Design decisions: brief inline comment above the relevant rule
- Token definitions: output in a separate `tokens.css` or Tailwind config block first, then reference them in component code
- If the output is long (> 200 lines), offer to split into separate files
