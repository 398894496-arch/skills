# Buckets cluster by user intent

Skills are grouped under `skills/<bucket>/` by the user's *intent* at the
moment they reach for a skill ("when I'm coding…", "when I'm writing…"),
not by skill type, lifecycle phase, or domain. A new bucket is only
justified when ≥2 skills share an intent distinct from existing buckets;
single-skill buckets are a smell — flatten until a second sibling exists.

Considered and rejected: flat (no buckets), by-lifecycle (`planning/`,
`implementation/`, `review/`), by-domain (`backend/`, `frontend/`).
Intent-based discovery matches how skills are actually triggered (by the
user describing what they're doing), so the folder structure mirrors the
mental access path.
