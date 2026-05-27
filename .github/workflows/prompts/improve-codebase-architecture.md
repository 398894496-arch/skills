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

3. Pick **one** top candidate that is not a loose duplicate of any prior
   proposal.

4. File it as a GitHub issue:

   ```
   gh issue create \
     --title "<concise title>" \
     --label source:architecture-review \
     --body "<full body>"
   ```

5. If every reasonable candidate is already covered by a prior
   `source:architecture-review` proposal, do **not** file an issue. Print
   `SKIPPED: <one-line reason>` and stop.

## Rules

- Read-only on the repo source. No commits. No edits to `CONTEXT.md`, ADRs,
  or source files. The only mutation allowed is `gh issue create` with the
  provenance label.
- One issue per run, maximum.
- No questions. There is no user.
