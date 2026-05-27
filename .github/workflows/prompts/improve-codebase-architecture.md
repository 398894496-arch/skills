# Daily architecture-review pass

You are running unattended in GitHub Actions. No user is watching. Do not ask
questions — make the call yourself.

## Task

1. List prior proposals labelled `source:architecture-review` (both open and
   closed) so you do not re-propose them:

   ```
   gh issue list --label source:architecture-review --state all --limit 100
   ```

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

5. File it as a GitHub issue. Include a "Sources" section listing the
   research links you used:

   ```
   gh issue create \
     --title "<concise title>" \
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
