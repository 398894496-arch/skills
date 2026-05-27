# Prompt: Re-tune an Existing UI

**Mode:** Aesthetic Refactor / Re-theme  
**Invoke when:** The user wants to change the visual direction, update styling,
or improve the look and feel of an existing page or component without
changing its underlying structure or behavior.

---

## Phase 0 — Gather Existing Context

Before proposing any changes, read and understand what exists. Ask:

```
To re-tune this UI effectively, I need to understand what's already here:

1. **Files to refactor** — Which file(s) should I work with?
   (Please share the code, or tell me the path.)

2. **What you want to change** — Is this a full visual direction change,
   a specific component update, or a targeted fix (e.g., "make it darker",
   "fix the typography", "the spacing feels wrong")?

3. **What must stay the same** — Are there brand assets, colors, or
   components I must not change? (Logo, existing design tokens, etc.)

4. **New direction (optional)** — Do you have a target aesthetic in mind?
   If not, I'll read the existing code, identify its current direction,
   and propose improvements or alternatives.

5. **Stack confirmation** — React, Next.js, Tailwind, or vanilla HTML/CSS?
   Any component libraries in use?
```

Wait for the user's response.

---

## Phase 1 — Existing Direction Analysis

Read the provided code and produce a brief analysis:

```
Current state analysis
──────────────────────
Inferred direction: [what the current UI seems to be aiming for]
Typography:         [what's there now — faces, scale, consistency]
Color system:       [tokens or hardcoded? Palette approach?]
Layout approach:    [grid, flexbox, spacing rhythm]
Motion:             [present or absent? Purposeful or decorative?]

Issues identified:
- [Specific problem 1 — e.g., "spacing is hardcoded in 11 places"]
- [Specific problem 2 — e.g., "type scale has no consistent ratio"]
- [Specific problem 3 — e.g., "Inter is used with no differentiated body face"]

Banned pattern flags:
- [Any patterns from the banned list present in the current code]
```

Present this analysis to the user before proposing changes.

---

## Phase 2 — Direction Proposal

If the user wants a new direction (or said "improve it"):

Propose **2–3 named aesthetic directions** that would work for this
specific product and its content. For each:

- **Name** it in 2–4 words
- Write 2 sentences on the feel
- Note what changes vs. what stays the same from the current design
- Flag any risk (e.g., "switching to a condensed display face may require
  adjusting heading hierarchy across all pages")

Apply the same self-critique as in the design prompt before presenting.

If the user already specified a direction, confirm it and proceed.

---

## Phase 3 — Incremental Refactor Plan

Before writing any code, present a brief refactor plan so the user can
validate scope:

```
Refactor plan for [Direction Name]
───────────────────────────────────
1. Replace hardcoded values with token layer (tokens.css / tailwind.config)
2. Swap typography to [display face] / [body face] with updated scale
3. Update color system to semantic tokens ([primary], [accent], [surface]…)
4. Refactor spacing to [scale name]
5. Update shadow/elevation to named scale
6. [Any additional direction-specific changes]

Estimated impact: [N] files, [N] components
```

Ask "Looks good?" before proceeding. Respect any scope changes the user
requests.

---

## Phase 4 — Token Layer First

Output the revised token layer before touching component code.
This ensures the user can review the palette and typography decisions
in isolation before they cascade through the codebase.

Use the same output format as the design prompt (Tailwind config extension
or `tokens.css`).

---

## Phase 5 — Component Refactor

Refactor each file or component:

- Replace hardcoded values with token references
- Apply the new type scale and font pairing
- Update color references to semantic token names
- Normalize spacing to the token scale
- Remove or replace any banned patterns found in Phase 1
- Preserve all existing functionality — do not change behavior, only styling
- Add a brief comment wherever a significant change was made and why

Output format: `diff`-style or full revised files, based on the size of
the change. For small changes (< 30 lines changed), use diff. For larger
refactors, output the full revised files.

---

## Phase 6 — Post-Refactor Audit

Apply the Design Review Checklist from SKILL.md and report:

```
✅ Post-refactor audit
──────────────────────
Direction purity:    no banned patterns remain ✓ / ✗
Contrast:            all text passes WCAG 2.2 AA ✓ / ✗
Token coverage:      [N]% of values now use tokens ✓ / ✗
Spacing consistency: all spacing uses token scale ✓ / ✗
Focus indicators:    present on all interactive elements ✓ / ✗

[Any ✗ items: fixed inline, described here]
```

---

## Conflict Handling

Same as `design.prompt.md`. If the user requests a pattern that conflicts
with the new direction or the banned pattern list:

```
⚠️ Design direction conflict
"[requested pattern]" conflicts with the [Direction Name] aesthetic /
is a common AI-generated UI signature.

Alternative that fits the current direction:
→ [specific alternative]

Say "proceed anyway" to override.
```
