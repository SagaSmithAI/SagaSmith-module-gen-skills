---
name: sagasmith-modulegen
description: "Create, revise, review, and finalize portable D&D 5e Module Packs for the current SagaSmith content-package v2 authoring contract. Use for one-shots, adventures, campaigns, sandboxes, generated module revisions, or evidence-bound module publication through module_draft and content_pack."
---

# SagaSmith Module Pack Authoring

Create one reviewed D&D source artifact, use `module_draft` as the only editable
workspace, and deliver one immutable `sagasmith.content-package` v2 Module Pack.
Treat Pack installation and campaign activation as optional operations outside
the default build result.

## Load the relevant references

- Read [pack-contract.md](references/pack-contract.md) before preparing Pack
  decisions or calling `module_draft`.
- Read [source-authoring.md](references/source-authoring.md) before writing or
  revising the Module Markdown and runtime manifest.
- Read [narrative-patterns.md](references/narrative-patterns.md) only when
  selecting an adventure structure or scaling a long work.
- Read [review-gates.md](references/review-gates.md) before mechanical import,
  finalization, and optional activation.
- Read [canonical-example.md](references/canonical-example.md) when an exact
  end-to-end call shape is useful.

## Keep one authority for each concern

- Own story meaning, source interpretation, stable semantic ids, Pack decisions,
  and final confirmation as the Agent.
- Let Core and D&D derive source bundles, scene boundaries, stable scene keys,
  chunks, assets, content reviews, actors, checksums, and the final descriptor.
- Let MCP own draft revisions, idempotency, authorization, immutable archives,
  import, activation, and progress remaps.
- Never hand-build a final Package descriptor, checksum, blob path, source
  receipt, actor card, or scene atlas.
- Never copy unrealized possibilities, secrets, endings, or future branches into
  CampaignMemory. Persist only outcomes that play actually realizes.

## 1. Preflight the authoring session

1. Confirm that the native tool list exposes `module_draft` and `content_pack`
   with the actions documented in [pack-contract.md](references/pack-contract.md).
2. Require Lobby phase and an authenticated campaign Owner or DM. Use the host-
   bound principal; do not invent or request a model-supplied identity.
3. Select an existing authoring campaign. Create one only when the user asks or
   when the surrounding workflow already authorizes campaign creation.
4. Default to **build only**. Do not import into another campaign or activate the
   result unless the user explicitly requests that additional outcome.
5. If the native schema differs from the bundled contract, stop and report the
   drift. Do not guess a compatibility path.

## 2. Establish the authoring ledger

Record these decisions before drafting prose:

- portable `pack_id`, semantic `version`, title, language, license, attribution;
- classification (`adventure` or `campaign`), supported D&D editions, and exact
  Pack dependencies when any;
- sourced party-size range, start/end levels, advancement modes, and
  pregenerated-character applicability;
- continuity series/order/state policy and activation policy;
- chapter and scene plan with unique, stable semantic headings;
- entities, secrets, clues, plot nodes, foreshadowing, branches, factions, and
  endings with stable lowercase ids;
- standard catalog groups: items, encounters, hazards, handouts, and mechanics;
- narrative dossiers and reachable endings.

Treat one-shot, short, medium, long, and sandbox as composition profiles, not
Pack schema values. Use `adventure` for bounded playable works. Use `campaign`
only for a continuing work with at least one explicit ending.

## 3. Design before expanding

1. Build a scene graph showing entrances, discoveries, consequential choices,
   failure paths, and endings.
2. Give every required revelation multiple reasonable discovery paths when the
   design depends on player discovery.
3. Separate standard D&D mechanics from module-specific prose. Use canonical
   mechanics and exact statblocks where required; do not make narrative-only
   actors mechanically executable.
4. Define what each faction and important NPC wants, fears, knows, and does if
   the party does nothing.
5. Keep unresolved narrative geometry as Agent-facing guidance. State dimensions
   and connections only when the authored source explicitly establishes them.
6. Review the ledger with the user before expanding a medium, long, or sandbox
   work when user review is available.

## 4. Author one canonical source

Write one UTF-8 Markdown source using this hierarchy:

```markdown
<!-- sagasmith-runtime-manifest
{ "schema_version": 1, "module_key": "stable-module-key" }
-->
# Chapter
## Scene
### Scene subsection
#### A1. Numbered room or location
```

Follow [source-authoring.md](references/source-authoring.md). In particular:

- include at most one valid runtime manifest;
- keep ids globally unique and stable across revisions;
- use `#` for chapters, `##` for scenes, `###` for subsections, and `####` for
  numbered rooms or locations;
- avoid repeated generic scene titles that produce unstable revision mapping;
- keep secrets and DM preparation in the authored source without claiming that
  Markdown can set scene ACLs; current D&D scenes default to `keeper` when the
  parser provides no visibility metadata;
- place player-facing portable material in the handouts catalog rather than
  promising unsupported scene-visibility editing.

Before starting the draft, integrate all sections into this one source and run
the source gate from [review-gates.md](references/review-gates.md).

## 5. Scale composition without splitting authority

For a small adventure, draft and integrate directly. For a large work, optionally
delegate independent chapter or region bodies after freezing the ledger.

When delegating:

- give each worker only its assigned ids, scene contract, global constraints,
  incoming state, and required outgoing state;
- prohibit workers from calling MCP, finalizing Packs, inventing dependencies,
  or creating separate runtime manifests;
- have the lead Agent merge all sections, resolve duplicate ids and headings,
  verify transitions and clues, and produce the single canonical source;
- use sequential drafting when delegation is unavailable. Do not maintain a
  platform capability matrix inside this Skill.

## 6. Start the editable draft

Call `module_draft(action="start")` once the generated source is internally
coherent. Supply either `source_path` or generated `name+content`, never both.
For generated work, normally supply `name`, complete `content`, `title`, and a
stable `source_key`.

Use one stable idempotency key for the exact start request. Retain the returned
`job_id`, state, draft revision, inspection, validation, and inactive
`module_id`. A successful start performs the mechanical first pass and normally
returns an `imported` editable draft.

If validation fails, repair the canonical source from diagnostics and start a
new corrected draft. Do not weaken the parser or fabricate evidence to preserve
one failed generated attempt.

## 7. Review from exact evidence

1. Use `module_draft(action="get")` to refresh the current draft before writing.
2. Use `module_draft(action="evidence", kind="chunks")` to read exact managed
   evidence and obtain source receipts.
3. Copy returned source receipts verbatim into sourced Pack decisions. Never
   invent a `chunk_hash`, page, source key, or citation.
4. Review the derived scene index, runtime-manifest advisories, source chunks,
   statblocks, assets, and progress-impact diagnostics.
5. Apply only current draft edits:
   - `source_text` for evidence-backed staged-PDF transcription repair;
   - `content` or `statblock` for reviewed structured content;
   - `asset` for managed source assets;
   - `actor` for already validated actor bindings;
   - `package` for manifest, catalogs, narrative, dependencies, metadata, or
     version decisions;
   - `advance` to rerun the mechanical path after an applicable repair.
6. Pass the latest `expected_revision` and a request-specific idempotency key on
   every write. Refresh the revision after every successful edit.
7. Record a concise note for each package decision so the final Pack preserves
   an auditable edit history.

Do not treat warnings as automatic blockers. Resolve or record warnings that
affect identity, source binding, scene structure, mechanics, required play
profile facts, or portability. Keep advisory presentation issues non-blocking.

## 8. Finalize the immutable Pack

Run every finalization gate in [review-gates.md](references/review-gates.md).
Then call `module_draft(action="finalize")` with the current `job_id`, portable
`pack_id`, final version if not already saved, and explicit Agent confirmation.

Write a confirmation note that states what was reviewed. Never confirm a draft
with unresolved required evidence, invalid mechanics, missing campaign endings,
or unknown Pack dependencies. Omit `include_package` unless the caller truly
needs the full descriptor in the response; the archive is the normal handoff.

Finalization must return a `compiled` draft and an immutable Pack artifact.
Inspect that artifact with `content_pack(action="get", kind="module")` and verify
identity, schema, checksum, finalization metadata, and component counts.

## 9. Deliver, then optionally install

Report:

- artifact handle, Pack id, version, system, schema, and checksum;
- source key/checksum and final draft job/revision;
- scene, asset, review, actor, catalog, dossier, and ending counts;
- material warnings and the Agent finalization note;
- whether the Pack is only built, imported, or active.

Stop after delivering the artifact by default.

When the user explicitly requests installation, call
`content_pack(action="import", kind="module")` with the artifact. Import remains
inactive. When the user also explicitly requests activation, refresh the target
campaign revision and call `content_pack(action="activate", kind="module")` on
the imported `module_id`.

For a replacement revision, review progress impact and supply only explicit
`from_scene_id` to `to_scene_key` remaps with reasons. Never activate the
mechanically imported draft module directly, guess a remap, or silently discard
realized progress.
