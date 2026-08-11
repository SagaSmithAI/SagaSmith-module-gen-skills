# Canonical build example

This compact example shows the current public sequence. Values are illustrative;
copy real evidence receipts and current revisions from tool responses.

## Contents

- [Author the source](#1-author-the-source)
- [Start the draft](#2-start-the-draft)
- [Obtain evidence](#3-obtain-evidence)
- [Save Package decisions](#4-save-package-decisions)
- [Finalize](#5-finalize)
- [Inspect and deliver](#6-inspect-and-deliver)
- [Optional installation](#7-optional-installation)

## 1. Author the source

```markdown
<!-- sagasmith-runtime-manifest
{
  "schema_version": 1,
  "module_key": "lantern-vault",
  "entities": [{"id": "npc:lantern-keeper", "kind": "npc", "name": "Lantern Keeper"}],
  "secrets": [{"id": "secret:lower-seal", "initial_knowers": ["npc:lantern-keeper"]}],
  "clues": [{"id": "clue:inverted-runes", "trigger": "inspect the inverted runes"}],
  "plot_nodes": [{"id": "plot:open-vault", "trigger": "open the lower vault", "consequences": []}],
  "foreshadowing": [{"id": "foreshadow:cold-flame"}],
  "branches": [{"id": "branch:restore-seal", "trigger": "offer to restore the seal", "consequences": []}]
}
-->
# Chapter One: The Broken Lantern
## Arrival at the Vault
This adventure is designed for three to five level 3 characters using milestone advancement.
No pregenerated characters are included. The party can reach level 4 by resolving the vault.
#### A1. Gatehouse
The gatehouse is 30 by 20 feet. Inverted runes show that the lower seal faces inward.
## The Lower Seal
The keeper offers a ritual that restores the seal if the party has not attacked her.
## Ending: Seal Restored
Restoring the seal closes the vault and ends the immediate threat.
```

## 2. Start the draft

```text
module_draft(
  campaign_id=<authoring campaign>,
  action="start",
  payload={
    name: "lantern-vault.md",
    content: <complete source above>,
    title: "The Lantern Vault",
    source_key: "lantern-vault"
  },
  idempotency_key="lantern-vault:start:v1"
)
```

Retain `job_id`, `module_id`, and revision. Require `state="imported"`.

## 3. Obtain evidence

```text
module_draft(
  campaign_id=<authoring campaign>,
  action="evidence",
  payload={job_id: <job>, kind: "chunks", query: "three to five", limit: 10}
)
```

Select the managed chunk that supports party size, levels, advancement, and
pregen applicability. Copy its returned `source_ref` exactly. Use more than one
receipt when those facts occur in different chunks.

## 4. Save Package decisions

```text
module_draft(
  campaign_id=<authoring campaign>,
  action="edit",
  payload={
    job_id: <job>,
    operation: "package",
    note: "Reviewed publication identity, play profile, catalogs, and endings.",
    version: "1.0.0",
    manifest: {
      title: "The Lantern Vault",
      classification: "adventure",
      compatibility: {
        editions: ["2014", "2024"],
        required_capabilities: ["module_pack_v2"]
      },
      play_profile: {
        party_size: {minimum: 3, maximum: 5, source_refs: [<receipt>]},
        starting_level: {value: 3, source_refs: [<receipt>]},
        expected_end_level: {value: 4, source_refs: [<receipt>]},
        advancement: {
          modes: ["milestone"],
          recommended: "milestone",
          source_refs: [<receipt>]
        },
        pregenerated_characters: {
          available: false,
          applicability: "Reviewed; none are included.",
          source_refs: [<receipt>]
        }
      },
      continuity: {
        series_id: null,
        order: null,
        continues_from: null,
        state_policy: {}
      },
      activation: {mode: "campaign_attach", default_active: false}
    },
    catalogs: {
      items: [],
      encounters: [],
      hazards: [],
      handouts: [],
      mechanics: []
    },
    narrative: {
      dossiers: [{
        id: "npc:lantern-keeper",
        name: "Lantern Keeper",
        role: "guardian",
        want: "restore the lower seal",
        fear: "the party will break it",
        secret_refs: ["secret:lower-seal"]
      }],
      endings: [{
        id: "ending:seal-restored",
        trigger: "restore the lower seal",
        consequences: ["the vault remains closed"]
      }]
    },
    dependencies: [],
    metadata: {
      language: "en",
      license: "private",
      attribution: "User-authored with SagaSmith"
    }
  },
  expected_revision=<current draft revision>,
  idempotency_key="lantern-vault:package:v1"
)
```

Refresh the returned revision.

## 5. Finalize

```text
module_draft(
  campaign_id=<authoring campaign>,
  action="finalize",
  payload={
    job_id: <job>,
    pack_id: "dnd5e.module.lantern-vault",
    confirmation: {
      confirmed: true,
      note: "Reviewed the complete source, runtime ids, scene structure, sourced play profile, catalogs, narrative, dependencies, and final Pack summary."
    }
  },
  idempotency_key="lantern-vault:finalize:v1"
)
```

Require `state="compiled"` and retain the returned artifact.

## 6. Inspect and deliver

```text
content_pack(
  action="get",
  payload={campaign_id: <authoring campaign>, kind: "module", artifact: <artifact>}
)
```

Verify v2 identity, checksum, confirmation metadata, and component counts. Report
the artifact as the build result. Do not import or activate unless requested.

## 7. Optional installation

```text
content_pack(
  action="import",
  payload={campaign_id: <target campaign>, kind: "module", artifact: <artifact>},
  idempotency_key="lantern-vault:import:<target>"
)
```

Refresh the target campaign revision. If activation was also requested:

```text
content_pack(
  action="activate",
  payload={campaign_id: <target campaign>, kind: "module", module_id: <imported module>},
  expected_revision=<fresh campaign revision>,
  idempotency_key="lantern-vault:activate:<target>"
)
```
