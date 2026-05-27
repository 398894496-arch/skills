# Prompt: Design Review and Accessibility Audit

**Mode:** Review / Audit  
**Invoke when:** The user asks to review, audit, check, or inspect an existing
UI for design quality, accessibility compliance, or adherence to web
interface guidelines.

---

## Phase 0 — Gather Files and Scope

If no files or patterns were provided with the request, ask:

```
Which files would you like me to review?

You can provide:
- A file path or glob (e.g., app/components/**/*.tsx)
- Pasted code directly in the chat
- A specific component name

You can also tell me the focus area if you want a targeted review:
- Accessibility only
- Visual design and hierarchy only
- Full review (default)
```

Wait for input before continuing.

---

## Phase 1 — Fetch Live Guidelines

Before running the review, fetch the latest Vercel Web Interface Guidelines:

```
Source: https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
Method: WebFetch
```

Apply the fetched rules alongside the internal checklist below. The live
guidelines take precedence over any built-in assumptions about best practices
that may have changed since this skill was authored.

If the fetch fails, proceed using the internal checklist only and note:
"⚠️ Could not fetch live Vercel guidelines. Review uses built-in checklist only."

---

## Phase 2 — Structural Read

Before applying any checklist items, read the provided code and note:

- What framework and stack is in use
- What the component or page appears to be (e.g., "a product listing page", "a login form")
- Whether a design system or token layer is present or absent
- Any immediately obvious issues (missing `alt`, unlabeled inputs, inline styles, etc.)

This context shapes how you interpret the checklist results.

---

## Phase 3 — Apply Checklist

Work through each checklist item systematically. For every failure, record:

```
[file]:[line] — [rule short name] — [what was found] — [suggested fix]
```

**Accessibility**

- [ ] A1. All `<img>` elements have appropriate `alt` text (non-decorative: descriptive; decorative: `alt=""`)
- [ ] A2. All `<input>`, `<select>`, `<textarea>` elements have a programmatically associated `<label>`
- [ ] A3. All interactive elements (buttons, links, custom controls) are keyboard-accessible and focusable
- [ ] A4. Focus indicators are visible; focused elements are not obscured by overlapping content (WCAG 2.4.11)
- [ ] A5. Focus indicators meet minimum visibility requirements (WCAG 2.4.13)
- [ ] A6. Color alone does not convey meaning (error states use icon/text, not only red)
- [ ] A7. `aria-label` or `aria-labelledby` is present on icon-only interactive elements
- [ ] A8. ARIA roles are used only where no native HTML equivalent exists
- [ ] A9. All animations include a `prefers-reduced-motion` override
- [ ] A10. Page-level landmark regions are present (`<main>`, `<nav>`, etc.)
- [ ] A11. Heading levels are sequential with no skipped levels
- [ ] A12. Authentication flows do not rely solely on cognitive tests (WCAG 3.3.8)

**Readability and Visual Hierarchy**

- [ ] R1. Body text contrast ≥ 4.5:1 against its background (normal size)
- [ ] R2. Large text and UI component contrast ≥ 3:1 (WCAG 2.2 AA)
- [ ] R3. Body line length is capped at approximately 75 characters
- [ ] R4. Line height is appropriate for each text size (not a single fixed value)
- [ ] R5. Type scale uses consistent named steps, not arbitrary pixel values
- [ ] R6. Font pairing has genuine role distinction (display vs. body vs. mono)
- [ ] R7. Heading and body fonts are not both generic system or ubiquitous defaults with no differentiation

**Layout and Spacing**

- [ ] L1. Spacing values reference a defined scale (tokens or Tailwind config)
- [ ] L2. Layout is verified at 320 px (mobile), 768 px (tablet), 1280 px (desktop)
- [ ] L3. No content is obscured by fixed headers without `scroll-margin-top` compensation
- [ ] L4. Safe area insets applied for sticky/fixed elements on mobile
- [ ] L5. Images use `aspect-ratio` to prevent layout shift

**Color and Tokens**

- [ ] C1. Color values use semantic token names, not raw hex literals
- [ ] C2. Dark mode (if present) is derived from the same token names as light mode
- [ ] C3. Shadow/elevation uses only named scale levels
- [ ] C4. No gray text on a colored background without verified contrast
- [ ] C5. No absolute black (`#000000`) or white (`#ffffff`) without deliberate rationale

**AI Slop Pattern Detection**

- [ ] S1. No full-bleed gradient hero (gradient + centered headline + 2 CTAs + screenshot)
- [ ] S2. No cardocalypse (cards within cards as primary layout pattern)
- [ ] S3. No oversized rounded icon above heading as sole visual element
- [ ] S4. No default purple/indigo gradient as brand accent without explicit justification
- [ ] S5. No glassmorphism used as primary structural element
- [ ] S6. No generic placeholder copy ("Lorem ipsum", "Get started", "Click here")
- [ ] S7. No elastic bounce animation on every interactive element

**Microcopy and Content States**

- [ ] M1. CTA button labels describe the action clearly
- [ ] M2. Error messages explain what went wrong and how to fix it
- [ ] M3. Loading states are defined for all data-fetching components
- [ ] M4. Empty states are defined for all list or collection components
- [ ] M5. No `placeholder` attribute used as a substitute for a visible label

---

## Phase 4 — Produce Report

Output the review report in this format:

```markdown
## Design Review: [filename or component name]

**Stack detected:** [react/nextjs/vanilla/etc.]
**Component type:** [what this appears to be]
**Aesthetic direction:** [detected aesthetic, or "no clear direction"]
**Live guidelines:** [fetched successfully / not available]

---

### 🔴 Critical — Must fix before shipping

[file]:[line] — [rule ID] — [what was found]
Fix: [specific, actionable suggestion]

[repeat for each critical issue]

---

### 🟡 Recommended — Should fix

[file]:[line] — [rule ID] — [what was found]
Fix: [specific, actionable suggestion]

---

### 🔵 Informational — Consider improving

[file]:[line] — [observation]
Suggestion: [optional improvement]

---

### Summary

- Critical issues: [N]
- Recommended fixes: [N]
- Informational notes: [N]
- WCAG 2.2 AA: Pass / Fail (based on A1–A12 above)
- AI slop patterns: [N] found / None found
```

**Severity classification:**

- **Critical:** Accessibility failures (A1–A12), contrast failures (R1–R2), broken keyboard navigation
- **Recommended:** Spacing not using tokens, missing states, weak type hierarchy, AI slop patterns
- **Informational:** Suggestions for stronger direction, copy improvements, non-breaking enhancements

---

## Phase 5 — Revised Code Output

After the report, output a revised version of the provided code with all
**Critical** and **Recommended** issues resolved.

Precede the revised code with:

```
## Revised Code
All critical and recommended issues have been applied.
Changes annotated with inline comments.
```

Annotate each fix with a brief inline comment:

```
/* FIX R1: contrast was 3.2:1, updated to near-black --color-on-surface */
```

If the code is long (> 150 lines), ask the user whether to output all files
at once or one at a time.

---

## Phase 6 — Follow-up

After delivering the report and revised code, offer:

```
Next steps I can help with:
1. Apply any Informational suggestions above
2. Build out missing states (loading, empty, error) for [component name]
3. Run the full design.prompt.md flow to establish a stronger direction
4. Check additional files — tell me the path or glob
```
