# holly-community-skills

Catalog of Python sub-agents discoverable by Holly's planner via DECISION 0058 Task 9d (community-skills-first preference) and DECISION 0066 (auto-registration on scaffold).

Holly fetches `catalog.json` on a 6h schedule, persists each entry to her local `skill_catalog` SQLite table and a palace memory under `skills/catalog/<source>`, and prefers `run_agent(name=...)` (with install-on-demand) over scaffolding a new agent when a request matches an existing entry (Jaccard score >= 0.35 on the request text).

## Trust model

Each entry carries a `pubkey` (hex Ed25519 public key). For Holly to auto-install without operator approval, the source's `auto_install` must be `true` AND the entry's `pubkey` must appear in the source's `trusted_pubkeys` list in the operator's `config/skill_sources.json`. Otherwise an `operator_requests` row is filed and the install pauses for explicit approval.

The placeholder `pubkey: "0000...00ff"` in this seed catalog is intentionally untrusted — operator-approval gating fires on every install until the operator decides which keys to trust.

## Schema

```json
{
  "name": "string (skill name, also python package dir)",
  "version": "string (semver)",
  "description": "string (one-line summary, shown in planner prompt)",
  "git": "string (https git URL clonable by Holly's git_ops)",
  "pubkey": "string (hex Ed25519 public key)",
  "tags": ["string", ...]
}
```
