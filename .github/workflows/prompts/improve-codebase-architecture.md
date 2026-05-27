# Daily architecture-review pass

You are running unattended in GitHub Actions. No user is watching. Do not ask
questions — make the call yourself.

## Scope

**In scope:** the `SKILL.md` files and their supporting markdown under
`skills/<bucket>/<name>/`. That is what this repo ships. Proposals
should improve the substance, structure, language, or accuracy of the
skills themselves — not the repo's meta-files (workflow, CONTEXT.md,
ADRs, README) and not the repo's tooling. If you cannot find a
candidate inside `skills/`, emit `SKIPPED`.

## Task

1. List prior proposals labelled `source:architecture-review` (both open and
   closed) so you do not re-propose them:

   ```
   gh issue list --label source:architecture-review --state all --limit 100
   ```

   Then **read the comments on closed issues** with
   `gh issue view <n> --comments`. The maintainer's pushback patterns
   ("you keep proposing X, here's why I reject it", "this is too
   speculative", "thin prescription") are your calibration signal. Note
   any recurring critique and avoid that failure mode this run.

2. Invoke the `/improve-codebase-architecture` skill to find one fresh
   deepening opportunity in this codebase. Read `CONTEXT.md` and any ADRs
   under `docs/adr/` first if they exist; treat ADRs as binding.

3. **Research before proposing.** The skills in this repo
   (`frontend-design`, `software-design`) encode evolving best practice.
   Before settling on a candidate, use `WebSearch` / `WebFetch` to check
   current thinking on the area you're proposing to deepen — design
   tokens, component patterns, module/seam boundaries, testing strategy,
   accessibility standards, etc. Cite 1–3 sources in the issue body so a
   future reader can see the basis for the proposal. Prefer primary
   sources (specs, framework docs, well-known authors) over listicles.

4. Pick **one** top candidate that is not a loose duplicate of any prior
   proposal.

5. File it as a GitHub issue. The body must satisfy:

   - **Title prefix.** Begin the title with `defect:` (broken link,
     dead reference, contradiction, factual error) or `deepening:`
     (architectural reframe, sharpened language, new structure). Open
     the body with a one-line justification of the category.
   - **Observed vs anticipated impact.** Separate what is *broken now*
     (with evidence — file paths, line numbers, ideally a quoted
     symptom from a real run) from what *could* go wrong on future
     runs. Do not let speculation read like observation.
   - **Concrete before/after.** Quote the current text (with file
     path) and write the exact replacement. No paraphrased intent. If
     a sentence/section gets moved or deleted, name it precisely.
   - **One recommendation, not a menu.** Make the call. Alternatives
     belong in a short "Rejected alternatives" footnote with the
     reason for rejection — never two equally-weighted "Option A /
     Option B" paths that punt the decision to the reader.
   - **Prescription proportional to diagnosis.** If you can't write a
     concrete fix that matches the weight of your problem statement,
     either sharpen the diagnosis or skip this candidate.
   - **Sources section** listing the research links you used.

   ```
   gh issue create \
     --title "defect: <concise title>" \
     --label source:architecture-review \
     --body "<full body, with a Sources section at the end>"
   ```

6. If every reasonable candidate is already covered by a prior
   `source:architecture-review` proposal, do **not** file an issue. Print
   `SKIPPED: <one-line reason>` and stop.

## Rules

- Read-only on the repo source. No commits. No edits to `CONTEXT.md`, ADRs,
  or source files. The only mutation allowed is `gh issue create` with the
  provenance label.
- One issue per run, maximum.
- No questions. There is no user.
