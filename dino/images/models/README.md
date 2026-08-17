# 3D model assets for the Dino Guide

Each species popup in `dino/index.html` uses [`<model-viewer>`](https://modelviewer.dev/)
to show a 3D model, and can optionally recolor it to match a camo built in
the [Skin Code Forge](../../../skincode-generator/).

## Where files go

```
dino/images/models/<key>.glb
```

`<key>` must match the species' `data-dino` attribute / `DINO_DATA` key in
`dino/index.html` (lowercase, e.g. `tyrannosaurus.glb`, `deinosuchus.glb`).
Currently wired up: `deinosuchus`, `tyrannosaurus`, `herrerasaurus`,
`dilophosaurus`, `ceratosaurus`, `troodon`, `omniraptor`, `pteranodon`,
`carnotaurus`. Other species have cards but no `DINO_DATA` entry yet — add
one there (with a `model` path) before dropping in a GLB for them.

## Recoloring: named materials

To let the Skin Code Forge camo apply to a model, give it up to six
materials named exactly:

| Material name | Skin code segment |
|---|---|
| `prefixTint` | Pattern Prefix |
| `underbelly` | Underbelly |
| `body` | Body |
| `flank` | Flank |
| `markings` | Markings |
| `maleDisplay` | Male Display |

At runtime, each is looked up by name and recolored via
`material.pbrMetallicRoughness.setBaseColorFactor([r, g, b, 1])`. You don't
need all six — a model with no separate male-display geometry can simply omit
that material, and it's skipped. Everything else about the material (metal/
roughness, textures, etc.) is left alone; only the base color factor is
overridden.

If no matching camo is found in localStorage for a species (i.e. the user
hasn't built one in Skin Code Forge, or built one for a different species),
the model just renders with whatever colors were authored into the GLB —
no override happens.
