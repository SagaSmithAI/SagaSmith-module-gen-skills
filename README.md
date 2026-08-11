# SagaSmith Module Pack Authoring

[English](README.md) | [中文](README-cn.md)

<p align="center">
  <img src="images/SagaSmith.png" alt="SagaSmith" width="180">
</p>

SagaSmith Module Generator is an AI-native D&D 5e authoring Skill for creating,
reviewing, and finalizing portable Module Packs under the current
`sagasmith.content-package` v2 contract.

It produces more than adventure prose. The workflow creates one canonical
Markdown source, stable runtime identities, structured Pack decisions, exact
source evidence, an auditable draft history, and one immutable Pack artifact.

## Current lifecycle

```text
brief and authoring ledger
→ canonical Module Markdown + runtime manifest v1
→ module_draft(start): mechanical first pass
→ module_draft(evidence/edit): Agent review and repair
→ module_draft(finalize): immutable Module Pack v2
→ content_pack(get): final verification and delivery
```

Import and activation are deliberately separate:

```text
content_pack(import) → inactive installed Module
content_pack(activate) → explicit campaign attachment
```

The default outcome is a finalized portable artifact, not an activated campaign
module.

## What the Skill authors

- one-shots, bounded adventures, campaigns, and sandboxes;
- stable entities, secrets, clues, plot nodes, foreshadowing, and branches;
- chapter, scene, subsection, and numbered-room Markdown structure;
- sourced party size, levels, advancement, and pregen applicability;
- item, encounter, hazard, handout, and mechanic catalogs;
- narrative dossiers, reachable endings, continuity, and dependencies;
- evidence-backed statblock, asset, and actor review decisions.

Core, D&D, and MCP remain authoritative for scene parsing, source bundles,
checksums, actor-card validation, immutable archives, revisions, authorization,
idempotency, and activation.

## Composition patterns

The Skill includes 25 reusable patterns such as Five-Room Dungeon, Node-Based,
Hexcrawl, Three-Act, Hero's Journey, Kishōtenketsu, Heist, Mystery,
Conspyramid, Faction Turn, Fish Tank, and Blorb. Patterns guide design; they do
not impose Package schema values or mandatory chapter counts.

Large works may delegate chapter or region drafting after the shared ledger is
frozen. The lead Agent still owns the only runtime manifest, integrates the only
canonical source, and performs the complete review and finalization sequence.

## Install

```bash
npx skills add SagaSmithAI/SagaSmith-module-gen-skills
```

Invoke the Skill as `$sagasmith-modulegen` and provide an adventure brief plus an
authoring campaign in Lobby when using SagaSmith D&D MCP.

## Ecosystem

| Repository | Role |
|---|---|
| **SagaSmith-module-gen-skills** | Module design and Pack authoring procedure |
| [SagaSmith-dnd-mcp](https://github.com/SagaSmithAI/SagaSmith-dnd-mcp) | Draft, evidence, finalization, Pack management, runtime state |
| [sagasmith-dnd](https://github.com/SagaSmithAI/sagasmith-dnd) | Deterministic D&D parsing, schemas, and mechanics |
| [sagasmith-core](https://github.com/SagaSmithAI/sagasmith-core) | System-neutral Package, source, document, and persistence primitives |
| [SagaSmith-dnd-skills](https://github.com/SagaSmithAI/SagaSmith-dnd-skills) | D&D play and DM procedures |
| [SagaSmith-agent](https://github.com/SagaSmithAI/SagaSmith-agent) | Multi-channel Agent host |

## License

Apache-2.0
