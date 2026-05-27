Skills are organized into bucket folders under skills/:

    engineering/ — daily code work

Every skill in engineering/must have a reference in the top-level README.md and an entry in .claude-plugin/plugin.json.

Each skill entry in the top-level README.md must link the skill name to its SKILL.md.

Each bucket folder has a README.md that lists every skill in the bucket with a one-line description, with the skill name linked to its SKILL.md.

## Agent skills

### Issue tracker

Issues live in GitHub Issues via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `CONTEXT.md` + `docs/adr/` at the repo root. See `docs/agents/domain.md`.
