# Banned Patterns — Tiered Catalogue

The catalogue of AI-slop UI patterns and their paired alternatives.
Loaded conditionally by `/frontend-design` during **Self-Critique** (Step 3
of the Iteration Loop) and **Post-Generation Audit** (Step 7).

Every row pairs a banned pattern with a named *instead reach for…*
alternative. The table is a **routing decision**, not a veto list — its job
is to steer generation toward direction-consistent choices.

Three tiers, by escalating severity:

- **BLOCK** — almost-always-slop. Refuse by default. Override requires a
  **brand-specific reason tied to the committed direction** (see
  `direction-doc-format.md`, the *Banned-pattern overrides* field). Plain
  "proceed anyway" is not enough.
- **WARN** — context-dependent. Single-line callout with the paired
  alternative; the user can accept or refine.
- **INFORM** — copy-quality or low-severity. Note in the post-generation
  report only; do not block.

---

## BLOCK

Override rule: each row below may only be used when the committed direction
in `docs/design/direction.md` lists a brand-specific reason for it under
*Banned-pattern overrides*. The skill names the conflict and refuses
until the override is recorded.

| Pattern | Why it's slop | Instead reach for… |
|---|---|---|
| Full-bleed gradient hero (gradient + centered headline + two CTAs + screenshot) | The single most common AI-SaaS signature; signals zero creative investment | A direction-specific hero: editorial type set against a calm surface, a product-first split layout, or a content-density hero that shows the actual product |
| Cardocalypse (cards within cards; every section wrapped in a rounded container) | Destroys hierarchy; turns the page into nested boxes of noise | Typographic hierarchy and whitespace; reserve cards for genuinely card-shaped data (rows, tiles, results) |
| Single saturated accent on near-black with no secondary palette (lime, teal, violet, cyan on `#0a0a0a`) | Merged from purple/indigo + neon-on-dark; the default "tech" reflex when no palette decision was made | A two- or three-hue palette anchored by the committed direction, with a desaturated base and one *earned* accent role |
| Untouched shadcn defaults (zinc palette, default radii, default focus rings, default font stack) | Ships the template as the design; signals no direction was chosen | An OKLCH palette and radius/shadow scale derived from the committed direction; override shadcn theme variables before any component lands |
| Bento grid as default feature-section layout | A 2024 trend recycled as a generic feature-grid; rarely fits the content it holds | A layout that follows the content shape: stacked editorial sections, a comparison table, or a single annotated screenshot |
| AI sparkle / wand / star-4 iconography as universal signifier | "✨" became the AI logo; using it signals derivative thinking | A glyph from the chosen icon family that reflects the product's actual verb (analyse, route, schedule) |
| Fabricated or irrelevant "Trusted by" logo bar | Manufactures credibility the product doesn't have; legally and ethically dubious | Real customer quotes with attribution, or omit social proof until it exists |
| Faux-terminal / code-block hero that doesn't demo the product | Decorative monospace as a vibe signal; the "code" is never real | An interactive snippet of the actual product, or no hero artifact at all |
| Section-cadence sameness (every section `py-24`, centered, eyebrow + h2 + paragraph + grid) | Same vertical rhythm across all sections turns the page into wallpaper | Asymmetric section variation: left-aligned dense sections beside centered focal ones; varied vertical spacing tied to information density |
| Motion as decoration (spring-on-every-click, Framer Motion `layout` on static cards, scroll-fade on every section) | Treats motion as noise; reads as a tech demo rather than a product | Motion that **confirms** an action, **guides** focus, or **reveals** hierarchy — and nothing else |
| Single-mode only (no light/dark pairing) | No `light-dark()`, no class-swap, no theme support; mechanically detectable; hard fail | A palette defined with `light-dark()` (or equivalent class-toggle) from the first token write, both modes tuned together |

---

## WARN

Single-line callout with the paired alternative. The user may accept,
refine, or override inline without amending the direction doc.

| Pattern | Why it's risky | Instead reach for… |
|---|---|---|
| Glassmorphism as a structural element (frosted panels carrying primary content) | Legibility degrades against real content backgrounds; ages quickly | Solid surfaces from the token palette; reserve translucent layers for true overlays (toasts, popovers) |
| Dotted / grid SVG background with radial mask behind hero | Generic "tech" texture; appears in dozens of AI-generated landing pages | A direction-consistent background: a single calm surface, a tasteful gradient tied to the palette, or a real product screenshot |
| Ambient gradient orbs / blurred blobs behind hero text | Decorative noise that competes with content; nothing about the product | Restrained color blocking or omit decoration entirely |
| Animated gradient on a headline keyword | Calls attention to itself, not the meaning; reads as a template tell | Type weight, optical sizing, or a brand-anchored color from the palette |
| Untuned light / dark mode (both present but one is an afterthought — white surfaces with one barely-changed accent, borderline contrast, no shadow tuning) | The mode that wasn't designed embarrasses the one that was | Tune surfaces, shadows, and accent saturation **per mode**; verify contrast in both |
| Forced-equal card heights via grid (content truncated or padded to fit) | The grid wins, the content loses | Auto-height cards with a `min-height` floor, or a masonry/staggered layout for genuinely uneven content |
| Emoji as decorative section anchor (🎯, 🚀, ⚡ above section headings) | Reads as Notion-doc-with-CSS; not a design choice | A small typographic eyebrow, a numeric prefix, or an icon from the chosen family |
| Generic 3D blob / Spline embed in hero | Decorative WebGL that signals "we tried something"; rarely tied to the product | A real product UI screenshot, an annotated diagram of the user's actual workflow, or no hero artifact |
| Generic "How it works" with 3 numbered steps that restate the headline | Filler content masquerading as structure | Either a concrete walkthrough with real screenshots, or remove the section |
| Oversized rounded icon (> 2rem) above a heading as the sole visual element | Pads the layout without adding information | Pair the icon with a meaningful illustration / data viz, or drop the icon and let the heading carry weight |
| Thick colored-border cards as the default container | The second-most-common AI card pattern after shadow-heavy cards | A token-defined elevation level or a subtle hairline border tied to the surface palette |

---

## INFORM

Note in the post-generation report. Do not block, do not warn inline.

| Pattern | Note | Instead reach for… |
|---|---|---|
| Empty CTA copy ("Get started", "Learn more", "Submit", "Click here") | Copy defect, not a design defect — but flag it | A verb that names the action and its object ("Start a 14-day trial", "Read the API reference", "Schedule the migration") |
