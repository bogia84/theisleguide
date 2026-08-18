# 3D model assets for the Dino Guide

Each species popup in `dino/index.html` can show a real [`<model-viewer>`](https://modelviewer.dev/)
3D model instead of the flat photo — but only if one is wired up. If a
species has no `model` entry, or the GLB fails to load, the popup falls
back to the real photo automatically. Nothing breaks either way.

## Where files go

```
dino/images/models/<key>.glb
```

`<key>` must match the species' `data-dino` attribute / `DINO_DATA` key in
`dino/index.html` (lowercase, e.g. `tyrannosaurus.glb`, `deinosuchus.glb`).

## Wiring one up

1. Drop the `.glb` file at the path above.
2. Add a `model: "images/models/<key>.glb"` field to that species' entry in
   `DINO_DATA` (in `dino/index.html`).

That's it — the popup will show the 3D model, with the photo used as the
`poster` (shown while the model loads). If you skip step 2, or the file is
missing/broken, the popup just shows the photo, same as any other species.

## Recoloring: named materials

To let the Skin Code Forge camo (`../../../skincode-generator/`) apply to
a model, give it up to six materials named exactly:

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
