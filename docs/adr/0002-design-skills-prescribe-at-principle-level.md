# Design skills prescribe at the principle level

Skills in this repo (`frontend-design`, `software-design`) teach how
to design software. They prescribe at the **principle level** —
concepts that hold across languages, frameworks, and stacks. Code
examples inside a skill are **illustrative sketches** of a
principle's shape, not rules to follow literally.

The same principle ("inject dependencies", "encode order in the
types", "build with named tokens not ad-hoc values") plays out
differently across stacks. Skill text that hardens one language's
idiom into the prescription either misleads readers in other stacks
or narrows the skill's reach. Issue #12 was a worked example: the
proposed fix added a TypeScript-specific branded-type pattern as the
remedy for "temporal coupling," when "encode order in the types" was
the load-bearing part.

**Consequence — match local section density.** When editing a skill,
match the density of the surrounding section. If sibling items
already ship code (issue #11: every other taxonomy in
`design-tokens.md` had CSS values), adding one to a missing sibling
restores symmetry and is correct. If the section is deliberately
terse (issue #12: the Anti-Patterns block in `testability.md` —
short diagnosis + one-line remedy per item), adding code to one item
signals that the specific snippet IS the rule, when it isn't.

This is design intent, not a ban. If a proposed code example
genuinely unlocks value prose cannot — surface it. The waste this
ADR avoids is proposals that contradict the spirit without checking
first.
