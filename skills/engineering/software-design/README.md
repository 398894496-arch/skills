# /software-design

`/software-design` is a **user-invoked** skill that runs after a PRD has
been broken into issues and before implementation begins. Its job is to
turn that backlog into a coherent design — named modules, located seams,
an explicit testing strategy, and TDD-ready issue bodies — so `/tdd` can
work one behavior at a time without rediscovering boundaries mid-flight.

The skill is **not model-invocable**. Invoke it explicitly with
`/software-design`. The skill self-checks its prerequisites and exits
early on work that doesn't need a design pass.

---

## Where this skill sits

The intended manual workflow is:

1. `/grill-with-docs` — sharpen domain vocabulary against the PRD; update
   `CONTEXT.md` and `docs/adr/`.
2. `/to-prd` and `/to-issues` — produce a backlog of vertical-slice issues.
3. **`/software-design`** — design modules and seams across the backlog;
   rewrite issue bodies into TDD-ready shape; write a Design Plan.
4. `/tdd` — implement one behavior at a time via red-green-refactor.

The skill is **not** automatic. The user decides when the backlog is
multi-module enough to warrant a design pass and invokes the skill at
that point. If it's invoked when the work is genuinely single-module,
the skill exits early and points the user at `/tdd` directly.

---

## What the skill thinks in

Modules, seams, and adapters. It's sharpest for backend services and
code with real I/O boundaries. Small CLIs, pure data pipelines, and UI
component trees usually hit the early-exit instead.

This is hexagonal-style thinking applied to a multi-issue backlog. The
goal is to make `/tdd` calmer and more disciplined by answering — before
implementation — *what module owns this behavior, where the test entry
point is, and what gets faked*.

---

## What good output looks like

- A module map using the project's own vocabulary from `CONTEXT.md`.
- Seams named in domain language (`PaymentGateway`, not `StripeClient`).
- Issue bodies rewritten so each one describes one observable behavior
  with a clear entry point, edge cases, and fake strategy.
- A Design Plan at `docs/design/<feature>.md` that captures modules,
  seams, testing strategy, invariants, and a one-line index linking to
  each rewritten issue. The issue tracker is authoritative for issue
  bodies; the Design Plan never duplicates them.

## What good output is not

- A heavyweight architecture spec.
- Framework-shaped layers or speculative abstractions.
- A long-lived reference document. The Design Plan is short-lived
  scaffolding; durable items (vocabulary, hard trade-offs) get extracted
  to `CONTEXT.md` and ADRs via `/grill-with-docs`.

---

## Files in this skill

| File | Role |
|---|---|
| `SKILL.md` | Workflow, self-check, anti-patterns, stale-handling. |
| `modules-and-seams.md` | Vocabulary for modules, seams, depth, deletion test, boundary placement, adapter strategy, decomposition anti-patterns. |
| `testability.md` | Interface-design rules that make tests natural. |
| `issue-shape.md` | The TDD-ready issue body format the skill writes. |
| `design-plan-format.md` | Template for `docs/design/<feature>.md`. |

---

## Worked example (sketch)

Raw `/to-issues` output for a checkout epic:

```
#1  Add a checkout endpoint
#2  Charge the customer's saved card
#3  Persist the order
#4  Send a confirmation email
```

After `/software-design`:

- **Modules**: `Checkout` (orchestrates the flow), `OrderConfirmation`
  (post-charge handling).
- **Seams**: `PaymentGateway` (charges), `OrderStore` (persists),
  `NotificationGateway` (sends confirmation).
- **Rewritten issue #1**: now names `Checkout` as the module, lists the
  entry point `checkout(cartId, paymentMethod)`, gives Given/When/Then
  acceptance criteria, names `FakePaymentGateway` and `FakeOrderStore`
  as the fakes for tests, and explicitly says *don't* test how the
  email markup is generated.
- **Design Plan**: `docs/design/checkout.md` records the module map,
  seam adapter strategies, invariants (*charging the same cart twice
  must be a no-op*), and links to the four rewritten issues.

`/tdd` now picks up issue #1 with no architectural inference to do.

---

## When this skill is the wrong tool

- Single-module changes, bugfixes, copy tweaks, one-liners.
- Backlogs with ≤2 issues touching one domain concept.
- Exploratory or throwaway work where boundaries are intentionally
  disposable.

In those cases go directly to `/tdd`. The skill will tell you so if you
invoke it anyway.
