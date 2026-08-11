# Module source authoring

Use this reference to create the single Markdown source consumed by the D&D
module parser. This file describes authoring discipline, not a second Package
schema.

## Contents

- [Document hierarchy](#document-hierarchy)
- [Runtime manifest v1](#runtime-manifest-v1)
- [Scene content contract](#scene-content-contract)
- [Spatial evidence](#spatial-evidence)
- [Secrets, knowledge, and visibility](#secrets-knowledge-and-visibility)
- [Integration pass for large works](#integration-pass-for-large-works)

## Document hierarchy

Use one canonical UTF-8 document:

```markdown
# Chapter One: The Broken Lantern

Chapter overview and operating context.

## Arrival at the Vault

Scene purpose, sensory details, actors, choices, clues, and consequences.

### Encounter

Use a subsection for material inside the scene.

#### A1. Gatehouse

Use a numbered room or location heading for spatial evidence.
```

The current D&D profile normally selects `##` headings as scenes. It may promote
`###` or `####` in source books with a dominant deeper hierarchy, but generated
content must use the stable hierarchy above. Avoid empty headings and repeated
generic scene names such as `Encounter` or `Room`.

The compiler derives scene stable keys from the complete heading path and
disambiguates repeats. Treat heading renames as identity changes that may require
explicit progress remaps when replacing an active Pack.

## Runtime manifest v1

Place at most one manifest near the beginning of the final Markdown:

```markdown
<!-- sagasmith-runtime-manifest
{
  "schema_version": 1,
  "module_key": "lantern-vault",
  "entities": [
    {
      "id": "npc:lantern-keeper",
      "kind": "npc",
      "name": "Lantern Keeper"
    }
  ],
  "secrets": [
    {
      "id": "secret:lower-seal",
      "truth": "The lower seal contains an imprisoned herald.",
      "initial_knowers": ["npc:lantern-keeper"]
    }
  ],
  "clues": [
    {
      "id": "clue:inverted-runes",
      "truth_ref": "secret:lower-seal",
      "source_scene": "chapter-one-the-broken-lantern-arrival-at-the-vault",
      "trigger": "inspect the inverted gate runes",
      "consequences": ["the inspecting actor may learn the seal was built inward"]
    }
  ],
  "plot_nodes": [
    {
      "id": "plot:open-lower-vault",
      "trigger": "break or restore the lower seal",
      "consequences": ["change access to the lower vault"],
      "failure_clock": {
        "segments": 4,
        "advance_when": "the party completes a rest without securing the gate"
      }
    }
  ],
  "foreshadowing": [
    {
      "id": "foreshadow:cold-flame",
      "setup": "the gate lantern burns without heat",
      "payoff": "the imprisoned herald bears the same flame"
    }
  ],
  "branches": [
    {
      "id": "branch:parley-with-keeper",
      "trigger": "offer to restore the seal before entering",
      "consequences": ["unlock the keeper's ritual assistance"]
    }
  ]
}
-->
```

The recognized collections are:

```text
entities, secrets, clues, plot_nodes, foreshadowing, branches
```

Keep all ids lowercase and globally unique. The current validator requires:

- `schema_version` equal to `1`;
- a stable lowercase `module_key`;
- every collection to be an array when present;
- every entry to be an object with a stable lowercase `id`;
- `secrets[].initial_knowers` to be an array when present;
- `clues`, `plot_nodes`, and `branches` to have a trigger;
- `plot_nodes[].consequences` and `branches[].consequences` to be arrays.

Use the same ids in dossiers, catalogs, handouts, endings, and prose. Keep an id
when its meaning survives a revision; create a new id when its meaning changes.

The runtime manifest records module possibilities and semantic identities. It
does not grant knowledge, advance a clock, realize a branch, set scene ACLs, or
write campaign continuity.

## Scene content contract

For every playable scene, state enough information for a DM to run it:

- purpose and situation on arrival;
- participating NPCs and their immediate goals;
- sensory evidence separated from hidden truth;
- likely actions, checks, and consequences without requiring one solution;
- clues with redundant discovery paths where the plot depends on them;
- transitions to other scenes and what changes if the party delays;
- encounter composition, tactics, retreat, and surrender behavior when combat is
  expected;
- rewards and persistent effects as possibilities, not already-realized facts.

Use canonical D&D terminology and mechanics. Include an exact statblock or bind a
validated actor when mechanical execution requires one. Do not invent a partial
statblock and expect MCP to complete it from narrative context.

## Spatial evidence

Use numbered `####` headings for actual rooms or locations. State dimensions,
doors, routes, elevation, cover, obstructions, and hazards only when authored.
Do not infer a tactical grid from narrative adjacency or draw connections merely
because headings are consecutive.

The Pack stores source spatial evidence. Runtime combat still chooses either:

- `grid`, with an authoritative compiled temporary map and coordinates; or
- `agent`, with no synthetic coordinates and explicit Agent spatial rulings.

## Secrets, knowledge, and visibility

Distinguish:

- authored truth: what the Module says can be true;
- initial knower: an authored entity that begins with the secret in the Module;
- discovered knowledge: an actor-specific runtime fact created only in play;
- public narration: an Agent decision made for the actual audience.

Do not pre-reveal secrets through handouts or summaries. Current generated D&D
scenes default to `keeper` when the parser supplies no visibility metadata, and
the public Package decision edit does not directly replace the derived
`scene_atlas`. Do not claim that a Markdown label changes scene authorization.

## Integration pass for large works

After merging delegated sections:

1. Verify every runtime id occurs with one meaning.
2. Resolve repeated chapter and scene headings.
3. Verify each scene's incoming and outgoing state.
4. Verify every essential clue has viable discovery routes.
5. Verify faction clocks and failure clocks agree across chapters.
6. Verify dossiers and endings use current ids.
7. Verify statblocks, DCs, rewards, and level expectations are coherent.
8. Regenerate the one canonical source; do not concatenate separate manifests.
