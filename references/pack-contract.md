# Current Module Pack contract

Use this reference when preparing Pack decisions or calling SagaSmith D&D MCP.
The authoritative final format is `sagasmith.content-package` schema version 2,
kind `module`, system `dnd5e`.

## Contents

- [Ownership](#ownership)
- [Public authoring facade](#public-authoring-facade)
- [Agent-authored Package decisions](#agent-authored-package-decisions)
- [Final artifact and optional runtime handoff](#final-artifact-and-optional-runtime-handoff)

## Ownership

The Agent supplies only authored source and explicit decisions. Core, D&D, and
MCP compile the final descriptor.

| Final Package area | Owner | Agent behavior |
|---|---|---|
| `format`, `schema_version`, `kind`, `system_id`, `checksum` | Core | Never hand-build |
| `id`, `version` | Agent decision, server validated | Keep portable and stable |
| `manifest` | Agent decision plus server identity fields | Submit through draft |
| `sources`, source chunks, normalized document | Core/D&D | Use returned evidence |
| `assets` and blob paths | MCP/Core | Attach managed assets only |
| `content_reviews` | Draft review operations | Never fabricate |
| `actors` | Validated actor bindings | Never write raw actor-card v3 |
| `content.scene_atlas` | Parser/compiler | Stabilize headings; do not edit directly |
| `content.catalogs`, `content.narrative` | Agent decisions | Submit as Package edits |
| `metadata.agent_finalization` | MCP from Agent confirmation | Confirm only after review |

Campaign ids, runtime character ids, permissions, ActorKnowledge, progress,
world state, random streams, branches, and snapshots are never portable.

## Public authoring facade

Use only:

```text
module_draft(start|get|evidence|edit|finalize)
content_pack(list|get|import|export|activate|deactivate|remove)
```

All authoring operations require Lobby. The host must bind the authenticated
principal. Writes require a stable idempotency key; draft edits must use the
latest applicable revision.

### Start

Generated source:

```json
{
  "campaign_id": "<authoring-campaign>",
  "action": "start",
  "payload": {
    "name": "<module>.md",
    "content": "<complete UTF-8 Markdown>",
    "title": "<title>",
    "source_key": "<stable-source-key>"
  },
  "idempotency_key": "<stable exact-request key>"
}
```

For a user-supplied managed source, replace `name+content` with `source_path`.
Never send both. A valid start mechanically inspects, validates, and imports an
inactive editable draft. Retain `job_id`, `module_id`, state, and revision.

### Get and evidence

Use `get` with no handle to list resumable drafts, or with `payload.job_id` for
the detailed current draft. Use `evidence` with `kind="chunks"`, optional query,
scene id, and a bounded limit. Evidence returns receipts shaped like:

```json
{
  "source_key": "<source-key>",
  "page": null,
  "chunk_hash": "<returned SHA-256>",
  "note": "<review note>"
}
```

This draft receipt is translated to the final Package `source_ref` with a managed
`chunk_key`. Copy the receipt; do not predict that final key.

### Package edit

Use `module_draft(action="edit")` with `payload.operation="package"`. The only
Package decision fields are:

```text
manifest, catalogs, narrative, dependencies, metadata, version
```

Include at least one decision and a concise `note`. Pass `expected_revision` and
refresh it after success. These edits remain mutable and are recorded in
`pack_edit_history` until finalization.

### Finalize

```json
{
  "campaign_id": "<authoring-campaign>",
  "action": "finalize",
  "payload": {
    "job_id": "<job-id>",
    "pack_id": "dnd5e.module.<portable-id>",
    "confirmation": {
      "confirmed": true,
      "note": "Reviewed source, scene structure, play profile, catalogs, narrative, and dependencies."
    }
  },
  "idempotency_key": "<finalize key>"
}
```

The draft must be mechanically imported. Confirmation must contain exactly
`confirmed` and `note`; the note must be meaningful. Finalization records the
bound reviewer, edit history, and draft revision, writes the immutable archive,
and moves the job to `compiled`.

## Agent-authored Package decisions

### Manifest

Supply exactly the required semantic fields below. The compiler adds package
identity and recomputes `content_summary`.

```json
{
  "title": "The Lantern Vault",
  "classification": "adventure",
  "compatibility": {
    "editions": ["2014", "2024"],
    "required_capabilities": ["module_pack_v2"]
  },
  "play_profile": {
    "party_size": {
      "minimum": 3,
      "maximum": 5,
      "source_refs": [{
        "source_key": "<copy from evidence>",
        "page": null,
        "chunk_hash": "<copy returned SHA-256>",
        "note": "<review note>"
      }]
    },
    "starting_level": {
      "value": 3,
      "source_refs": [{
        "source_key": "<copy from evidence>",
        "page": null,
        "chunk_hash": "<copy returned SHA-256>",
        "note": "<review note>"
      }]
    },
    "expected_end_level": {
      "value": 4,
      "source_refs": [{
        "source_key": "<copy from evidence>",
        "page": null,
        "chunk_hash": "<copy returned SHA-256>",
        "note": "<review note>"
      }]
    },
    "advancement": {
      "modes": ["milestone"],
      "recommended": "milestone",
      "source_refs": [{
        "source_key": "<copy from evidence>",
        "page": null,
        "chunk_hash": "<copy returned SHA-256>",
        "note": "<review note>"
      }]
    },
    "pregenerated_characters": {
      "available": false,
      "applicability": "Reviewed; none are included.",
      "source_refs": [{
        "source_key": "<copy from evidence>",
        "page": null,
        "chunk_hash": "<copy returned SHA-256>",
        "note": "<review note>"
      }]
    }
  },
  "continuity": {
    "series_id": null,
    "order": null,
    "continues_from": null,
    "state_policy": {}
  },
  "activation": {
    "mode": "campaign_attach",
    "default_active": false
  }
}
```

Every play-profile subsection must have at least one real source receipt.
Unknown party size, levels, advancement, or pregen applicability cannot be
finalized. A `campaign` classification also requires at least one ending.

### Catalogs

Use arrays for every catalog. Prefer the standard groups:

```json
{
  "items": [],
  "encounters": [],
  "hazards": [],
  "handouts": [],
  "mechanics": []
}
```

Give catalog entries stable ids and source receipts where they assert source-
bound facts. Put player-facing portable documents in `handouts`; a handout does
not automatically reveal itself during play.

### Narrative

Supply both required arrays:

```json
{
  "dossiers": [
    {
      "id": "npc:lantern-keeper",
      "name": "Lantern Keeper",
      "role": "guardian",
      "want": "keep the lower seal intact",
      "fear": "the party will mistake caution for betrayal",
      "secret_refs": ["secret:lower-seal"]
    }
  ],
  "endings": [
    {
      "id": "ending:seal-restored",
      "trigger": "the party restores the lower seal",
      "consequences": ["the vault remains closed"]
    }
  ]
}
```

Inner dossier and ending semantics are Agent-authored module data. Keep their
ids aligned with the runtime manifest and source text.

### Dependencies

Each dependency must contain exactly:

```json
{
  "kind": "core_rules",
  "id": "<pack-id>",
  "version": "<exact-version>",
  "checksum": "<exact lowercase SHA-256>",
  "optional": false
}
```

Discover dependencies from installed/finalized Packs. Never invent a checksum.
Use an empty array when the Module has no portable Pack dependency.

### Metadata

Use metadata for publication and authoring decisions such as language, license,
attribution, authorship, and Pack-specific rulings. Do not store campaign state
or duplicate server-derived fields.

## Final artifact and optional runtime handoff

Inspect the finalized artifact with:

```text
content_pack(get, kind="module", artifact=<artifact>)
```

Build completion ends here by default.

To install explicitly, call `content_pack(import)` with exactly one artifact or
source path. The imported Module remains inactive. To activate explicitly,
refresh the target campaign revision and call `content_pack(activate)` with the
imported `module_id` and that revision.

Activation accepts progress remaps only as exact objects containing
`from_scene_id`, `to_scene_key`, and `reason`. Use them only when replacing an
active revision after reviewing reported progress impact.
