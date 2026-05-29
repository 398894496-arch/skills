# Skill-improvement workflows run AFK and propose via issues

Two recurring workflows consume the [`agent-research`](https://github.com/dividedby/agent-research)
knowledge base to improve this repo: a **self-improvement** workflow that
refines existing skills against how skilled practitioners build agents, and a
**gap-scanner** that reads the maintainer's *other* repos to propose net-new
skills. Both run unattended on an always-on VPS.

Neither ships as a skill. "Improve *my own* skills from *my own* KB" and
"propose skills for *my own* repos" is narrow, personal machinery — exactly what
[ADR 0001](./0001-buckets-cluster-by-user-intent.md) and the repo's ethos keep
*out* of the published plugin. They are **run-books** (see `CONTEXT.md`): cron-
driven prompts the AFK agent follows, living in this repo (co-located with what
they edit) but never registered in `plugin.json`. The self-improvement run-book
may *invoke* `improve-codebase-architecture` as a step; it is not a cron over it.

**Producer / decider split.** The VPS runs only the *producer* phase — read,
analyze, write up. A human runs the *decider* phase — review, merge. The line
between them is the control that makes unattended operation safe:

- The VPS **may auto-commit analysis/bookkeeping** — specifically the
  integration map, which is C's own reasoning baseline, not a change to a skill.
- The VPS **may not auto-merge changes to skills themselves.** Each run surfaces
  its single highest-value proposal as **at most one issue** (zero is fine) on
  the tracker, tagged with a label (e.g. `self-improvement`) the run-book also
  reads to avoid re-filing. The existing triage labels gate the merge.

The one-issue-per-run cap is deliberate: it forces each run to pick *the* best
proposal rather than spraying a backlog, and keeps the human review trickle
digestible.

**The gap-scanner files into a public tracker.** This repo is public, so its
issues are public, but the gap-scanner reads **private** repos. Its issues must
therefore describe the recurring *need* and the proposed skill in **generalized
terms — never quoting or paraphrasing proprietary code.** This aligns with the
ethos anyway: a skill worth proposing is broadly useful, so its proposal should
read as broadly useful. The scanner reads private repos read-only, shallow, and
cleans up after each run; the repo list is an explicit curated config, not
auto-discovery of everything the maintainer owns.

**Amendment (#26): the sanitizer guard covers self-improvement too.** This ADR
originally framed the structural sanitizer around the gap-scanner, because that
was the workflow obviously reading private *repos*. But self-improvement also
reads a private source — the `agent-research` KB — and (since #19) files issues
whose title/body are derived from it into this same public tracker. That is the
same private-source → public-tracker risk shape, so the guard applies to both:
each run-book runs its filed `title + body` through `check()` (always passing
the configured `private_markers`) before filing, dropping any draft that trips.
The guard is structural and necessary-not-sufficient (see `sanitizer.py`); prose
discipline still matters for private identifiers the markers don't cover.

These workflows are **independent jobs**, not a pipeline. The KB is a *source*
they consult — often the first one — not a gate: a run with no KB change can
still find a proposal from the repo itself, its own integration map, or the
maintainer's other repos.

**Rejected alternatives.** Shipping them as skills (too narrow for the plugin).
One orchestrated B→C→D pipeline (rejected — independent jobs; the KB is a source,
not a trigger). Auto-merging skill changes (no human gate; an AFK agent editing
the published plugin unsupervised is the exact risk this split avoids).
