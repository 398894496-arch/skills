# Prompt: Design a New UI from Scratch

**Mode:** Greenfield Design  
**Invoke when:** The user asks to build, create, or design a new UI, page, component, or application interface.

---

## Phase 0 — Information Gathering

Before taking any design or code action, collect the following. Ask as a
short, numbered list — do not ask questions one at a time.

If the user has already provided some of these details in their request,
pre-fill those answers and only ask for what is missing.

```
I need a few details before I start designing:

1. **Stack** — Are you using React, Next.js (App Router or Pages), Tailwind CSS,
   or vanilla HTML/CSS/JS? Any component libraries (shadcn/ui, Radix, etc.)?

2. **What you're building** — Describe the product, screen, or component in
   one or two sentences. Who uses it, and what is their primary task?

3. **Key content and sections** — What are the main elements on screen?
   (e.g., navigation, hero section, data table, form, sidebar)

4. **Brand constraints** — Do you have existing colors, fonts, or a logo?
   If yes, share them. If no, I'll build from scratch.

5. **Aesthetic direction (optional)** — Any references, adjectives, or
   design sensibilities you want? (e.g., "editorial and dense like a
   newspaper", "minimal like Linear.app", "warm and tactile"). Leave
   blank and I'll propose 2–3 directions for you to choose from.

6. **Constraints** — Anything I must work within? (Existing design tokens,
   a specific Tailwind config, integration with existing components, etc.)
```

Wait for the user's response before continuing.

---

## Phase 1 — Aesthetic Direction

Based on the gathered inputs, propose **2–3 named aesthetic directions**.
For each direction:

- **Name** it in 2–4 words (e.g., "Warm Editorial Serif", "Precision Utility Mono", "Tactile Dark Matter")
- Write **2–3 sentences** describing the visual and interactive feel
- List the proposed **type pairing** (display face + body face + optional mono)
- List the **palette approach** (mood, not specific hex yet)
- Describe the **layout and spacing personality** (dense, airy, structured, organic…)
- Describe the **motion character** (fast and functional, deliberate and cinematic, static…)

Then apply the internal direction self-critique:

For each proposed direction, silently verify:

- [ ] Avoids all banned default patterns (no purple gradient hero, no cardocalypse, no generic SaaS layout)
- [ ] Type pairing has genuine role distinction (different optical weight, different classification)
- [ ] Palette can achieve WCAG 2.2 AA contrast in both light and dark
- [ ] Layout works at 320 px
- [ ] Motion character is purposeful, not decorative

If any direction fails 2+ checks, replace it with a revised option before presenting.

Present the directions and ask the user to choose one, or request changes.
Do not proceed to code until a direction is confirmed.

Confirm with: "Proceeding with **[Direction Name]**. I'll stay consistent with
this direction throughout."

---

## Phase 2 — Token Definitions

Before writing any component code, output the design token layer.

For Tailwind CSS projects, output a `tailwind.config.ts` extension block:

```ts
// tailwind.config.ts — [Direction Name] tokens
export default {
  theme: {
    extend: {
      colors: {
        /* semantic color tokens */
      },
      fontFamily: {
        /* display, body, mono */
      },
      fontSize: {
        /* named scale */
      },
      spacing: {
        /* spacing scale */
      },
      boxShadow: {
        /* named elevation levels */
      },
    },
  },
};
```

For vanilla or non-Tailwind projects, output a `tokens.css` file using
CSS custom properties.

Annotate each significant token with a brief comment explaining the choice.

---

## Phase 3 — Component/Page Generation

Generate the requested UI following all behavioral rules in SKILL.md:

- Use only token references — never hardcode colors, font sizes, or spacing values
- All interactive elements must have keyboard support and visible focus indicators
- Include `prefers-reduced-motion` media query for all animations
- Use semantic HTML elements before reaching for ARIA
- Ensure responsive behavior at mobile (320 px), tablet (768 px), desktop (1280 px)
- Add brief inline comments on non-obvious design decisions

Structure the output:

1. `tokens.css` or `tailwind.config.ts` extension (if not already output)
2. Component file(s) in the target stack
3. Any companion CSS/style file

---

## Phase 4 — Post-Generation Self-Audit

Immediately after generating code, apply the Design Review Checklist from
the SKILL.md review mode. Report the results:

```
✅ Post-generation audit
────────────────────────
Contrast:         all text ≥ 4.5:1 on backgrounds ✓ / ✗ [details]
Focus indicators: present on all interactive elements ✓ / ✗ [details]
Spacing:          all values use token scale ✓ / ✗ [details]
Motion:           prefers-reduced-motion respected ✓ / ✗ [details]
Direction purity: no banned default patterns present ✓ / ✗ [details]

[If any ✗: "Fixed: [description of fix applied]"]
```

Apply fixes inline before presenting the final output. Do not present code
that fails its own audit.

---

## Conflict Handling

If at any point in the conversation the user requests a pattern that conflicts
with the committed aesthetic direction or the banned pattern list, respond:

```
⚠️ Design direction conflict
"[requested pattern]" conflicts with the [Direction Name] aesthetic / is a
common AI-generated UI signature.

Alternative that fits the current direction:
→ [specific, concrete alternative with a one-line rationale]

Say "proceed anyway" if you want to override and I'll implement it.
```
