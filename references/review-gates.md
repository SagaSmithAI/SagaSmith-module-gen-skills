# Module Pack review gates

Run these gates at the named trust boundaries. Block only for authority,
evidence, identity, portability, or mechanically indispensable failures.

## Contents

- [Gate A: before draft start](#gate-a-before-module_draftstart)
- [Gate B: after mechanical start](#gate-b-after-mechanical-start)
- [Gate C: before Package finalization](#gate-c-before-package-finalization)
- [Gate D: after finalization](#gate-d-after-finalization)
- [Gate E: optional import and activation](#gate-e-optional-import-and-activation)
- [Retry rules](#retry-rules)

## Gate A: before `module_draft(start)`

- The source is one UTF-8 Markdown document.
- Exactly zero or one runtime manifest exists; when present, it is valid JSON v1.
- Runtime ids are lowercase, stable, globally unique, and aligned with prose.
- `#` chapters, `##` scenes, `###` subsections, and `####` numbered locations form
  a consistent hierarchy.
- Scene titles are meaningful and do not rely on repeated generic labels.
- Essential revelations have viable clue paths.
- All expected combatants have exact mechanics or are explicitly narrative-only.
- Dimensions and connections are stated only where authored.
- The authoring ledger includes Pack identity, classification, editions, play
  profile, continuity, catalogs, narrative, dependencies, and publication data.

Repair failures in the canonical source before starting the draft.

## Gate B: after mechanical start

- The draft has a retained `job_id`, inactive `module_id`, and current revision.
- State is `imported` before finalization work.
- Inspection reports the expected parser profile and runtime manifest.
- Validation is valid; errors are empty.
- Scene count, chapter assignments, stable keys, and chunk boundaries match the
  authored design.
- No empty, swallowed, or accidentally split scenes exist.
- Spatial locations and connections are conservative.
- Statblock, OCR, asset, and progress-impact diagnostics have been reviewed.

If generated Markdown cannot be repaired through an applicable draft operation,
repair the canonical source and start a corrected draft rather than adding a
parser heuristic for that single work.

## Gate C: before Package finalization

### Identity and evidence

- `pack_id` and version are portable, stable, and intentional.
- Every required play-profile subsection has a copied evidence receipt.
- No source key, chunk hash, page, dependency checksum, or actor identity was
  invented.
- Every edit used the then-current draft revision and a request-specific
  idempotency key.

### Package decisions

- Manifest contains title, classification, compatibility, play profile,
  continuity, and activation.
- Catalog values are arrays; standard groups are present or intentionally empty.
- Narrative contains both `dossiers` and `endings` arrays.
- A `campaign` contains at least one reachable ending.
- Dependencies have exact kind, id, version, checksum, and optional flag.
- Metadata contains publication decisions, not runtime campaign state.

### Playability and semantics

- Required mechanics are complete and use the intended D&D edition.
- Clues, secrets, branches, dossiers, handouts, and endings share stable ids.
- Initial knowers do not imply that player characters know a secret.
- All endings and persistent consequences remain possibilities until play.
- Optional portraits, presentation polish, or nonessential card fields do not
  block an otherwise mechanically complete Pack.

### Confirmation

- The Agent has reviewed the complete current revision.
- The confirmation note names the reviewed surfaces.
- No unresolved player choice, missing permission, stale revision, conflicting
  required evidence, or indispensable mechanic remains.

## Gate D: after finalization

Inspect the artifact through `content_pack(get)` and verify:

- `format` is `sagasmith.content-package` and `schema_version` is `2`;
- `kind` is `module` and `system_id` is `dnd5e`;
- id, version, manifest identity, checksum, and finalization metadata agree;
- source, asset, review, actor, scene, catalog, dossier, and ending counts are
  plausible;
- the draft state is `compiled`;
- campaign state, permissions, progress, knowledge, random state, branches, and
  snapshots are absent.

Build completion ends at this gate unless installation was explicitly requested.

## Gate E: optional import and activation

- Import exactly one finalized artifact or source path with kind `module`.
- Verify the imported `module_id`; do not activate the mechanically imported
  authoring draft.
- Refresh the target campaign revision immediately before activation.
- Review replacement progress impact.
- Supply explicit, reasoned progress remaps for removed realized scenes; never
  guess or discard progress.
- Confirm the resulting active module identity and next legal native call.

## Retry rules

- Reuse an idempotency key only for an exact retry of the same request.
- Use a new key when any payload field changes.
- Refresh the draft or campaign revision after a stale-revision failure.
- Do not convert an authorization, evidence, or revision failure into a warning.
