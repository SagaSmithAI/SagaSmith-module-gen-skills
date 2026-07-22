# SagaSmith Runtime Module Contract

Read this reference before generating or revising a module intended for the
SagaSmith D&D MCP. The manifest describes immutable module possibilities; it
does not claim that any outcome has happened in a campaign.

## Embedded manifest

Place exactly one JSON object in an HTML comment near the start of the final
Markdown. Keep ids lowercase and stable across revisions. The D&D parser validates
the manifest during inspect/validate and stores it with the immutable module
revision.

```markdown
<!-- sagasmith-runtime-manifest
{
  "schema_version": 1,
  "module_key": "keep-on-borderlands",
  "entities": [
    {
      "id": "npc:keeper",
      "kind": "npc",
      "name": "Keeper Harbin",
      "visibility": "keeper"
    }
  ],
  "secrets": [
    {
      "id": "secret:keeper-oath",
      "truth": "Harbin swore to protect the sealed gate.",
      "initial_knowers": ["npc:keeper"],
      "visibility": "keeper"
    }
  ],
  "clues": [
    {
      "id": "clue:broken-seal",
      "truth_ref": "secret:keeper-oath",
      "source_scene": "chapter-one:gatehouse",
      "trigger": "inspect the cracked gate seal",
      "consequences": ["the inspecting actor may learn the oath existed"]
    }
  ],
  "plot_nodes": [
    {
      "id": "plot:open-gate",
      "trigger": "repair or break the seal",
      "consequences": ["change access to the lower keep"],
      "failure_clock": {"segments": 4, "advance_when": "the party delays a watch"}
    }
  ],
  "foreshadowing": [
    {
      "id": "foreshadow:red-ravens",
      "setup": "red ravens gather over the gatehouse",
      "payoff": "the oath-breaker's herald uses the same sign"
    }
  ],
  "branches": [
    {
      "id": "branch:parley",
      "trigger": "offer terms before opening the gate",
      "consequences": ["unlock negotiation scene", "delay the failure clock"]
    }
  ]
}
-->
```

The recognized collections are `entities`, `secrets`, `clues`, `plot_nodes`,
`foreshadowing`, and `branches`. Every entry requires a globally unique `id`.
Clues, plot nodes, and branches require an explicit `trigger`; plot nodes and
branches use a list of `consequences`. Secrets list only their initial knowers.
Use `public`, `party`, or `keeper` visibility; default unrevealed material to
`keeper`.

## Runtime boundary

- Import stores definitions and provenance, not CampaignMemory facts.
- `module_set_progress` stores branch-local progress without editing definitions.
- A clue becomes ActorKnowledge only for actors who actually discover or receive
  it during play.
- A possibility becomes a world fact only after the scene resolves it.
- The DM Skill persists realized outcomes through one `continuity_commit`, citing
  the event and relevant module ids.
- Module revisions never rewrite already-realized facts or actor knowledge.

## Managed import lifecycle

Use `module_import` in this exact order: `stage`, `inspect`, `validate`, `ingest`,
`activate`. Pass generated `name+content` to `stage`, carry the returned `job_id`,
use a different stable idempotency key for each stage, and pass the current
campaign revision to `activate`. Review parser warnings, scene keys, manifest
errors, checksums, and progress impact before activation. Read runtime content
through `module_query`, not a local generation file.
