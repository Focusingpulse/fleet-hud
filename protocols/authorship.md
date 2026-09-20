---
name: authorship protocol
description: Fleet authorship and lineage standard. Every artifact an agent produces carries an authorship block naming the authoring agent by agent_id (never display name) plus the job and lineage that produced it. Use when writing any artifact to living-library or a published surface, when recording a claim, or when a sub-agent produces work. Implements the Veracity Ring provenance principle.
---

# Authorship Protocol (fleet-wide)

**Every artifact you write carries an `authorship` block. No exceptions on
ground-truth artifacts.**

## Source of the principle

This protocol implements the provenance principle from **Russell M. Wright's
*Entity Veracity*** — the same author cited elsewhere in fleet doctrine:

- **Source:** `https://entity-veracity.super-intelligent.ai/` (per-chapter sites
  at `chapter<N>-entityveracity.super-intelligent.ai`)

**Why the link is here (added 2026-09-20):** the protocol previously cited "the
Veracity Ring provenance principle" without naming or linking the source. A
framework cited in doctrine without a link is one the fleet cannot check — and a
private, unlinkable citation is worse, because it cannot be falsified at all. On
2026-09-20 two agents spent a round trip on a question one URL would have settled,
and a term ("Grounded Claim") was found in doctrine that is not a term in the
source. **Cite the source, so the citation can be checked.**

**Before attributing any term to this framework, check
`FRAMEWORK-TERM-INDEX.md`** — it lists each term we use, whether it is in the
source, and where. A term list with locations falsifies a citation without
reproducing the source's content.

## The block

```json
"authorship": {
  "agent_id": "<your agent id>",
  "agent_name": "<your display name>",
  "job": "<the cron/job that produced this>",
  "lineage": "<agent_id> -> <job> -> <artifact slug>",
  "authored_at": "<YYYY-MM-DD>",
  "note": "Authored by an AI agent. agent_id is the identity; the display name is for humans only."
}
```

For markdown artifacts, put the same fields in frontmatter:

```yaml
---
name: <artifact name>
description: <what it is>
author_agent_id: agent-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
author_agent_name: <display name>
author_job: <cron/job name>
authored_at: 2026-09-19
---
```

## Why agent_id and not the name

**Display names collide by design in this fleet.** Wizard, Scout, and Librarian are
the popular ones. In our own roster:

- `wizard` is **both** a registry agent **and** a ledger job
- `Scout` is a pod **plus six jobs**

If you record authorship by name, "Scout wrote this" is unfalsifiable — it could be
any of seven things. **Only the `agent_id` carries provenance.**

This is our local application of the provenance principle in the Entity Veracity
work: *who said it first, who can prove it, who signed the receipt.* Our authors are
often AI. That does not remove the requirement — it makes it more important. An AI
author with no ID is an author with no accountability.

## Sub-agents

A **sub-agent** is an agent spawned BY another agent. Sub-agents are runtime
artifacts, not fleet members — do not list them as authors.

**When a sub-agent produces work, the SPAWNING agent is the author.** Attribute to
the parent `agent_id` and note the spawn in the lineage:

```
agent-<parent> -> <job> -> (spawned sub-agent) -> <artifact>
```

## What counts as authorship

- **Your own output** — you are the author. Use your own agent_id.
- **Content you ingested from the web** — you are NOT the author. You are the
  *cataloger*. Record `author_agent_id` as yourself and add the source's own
  provenance separately (`source_url`, `source_author` if known). Never let an
  ingested document's identity masquerade as ours.
- **A claim you recorded** — you are the author of the *record*, and the claim's
  own origin goes in `sources`. Both must be present.

## Applies to

Ground-truth artifacts — these are non-negotiable:
- `living-library/synthesis/death-certificates/*.json`
- `living-library/synthesis/ground-truth/claims.jsonl`
- `living-library/synthesis/validations/`
- `living-library/synthesis/quest-queue/`
- `living-library/declassified/`

Also: AFLinks published pages, quest cards, dossiers, and any report that carries a
claim. Scraped sources and archives are third-party content — record the source's
provenance, not ours.

## Status

Backfilled on death certificates and claims.jsonl 2026-09-19. **Not yet enforced** —
no check currently rejects an artifact that omits the block. Until a check exists,
this is a discipline, and discipline decays. If you are a cron producer, emit the
block every run.

**Full standard:** `living-library/synthesis/ground-truth/AUTHORSHIP.md`
**Machine-readable rule:** `cron-coordination/fleet-registry.json` → `taxonomy.authorship`

---

*Generated from `skills/authorship-protocol/SKILL.md` in the fleet coordination repo (source of truth). Do not edit this copy - edit the source and let `protocols_render.py` republish it.*
