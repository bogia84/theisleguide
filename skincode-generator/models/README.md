# 3D model assets for Skin Code Forge

Drop one folder per species in here and the generator picks it up automatically —
no code changes needed. Species with no folder just show the existing 2D preview.

## Folder layout

```
models/
  <Species>/          e.g. models/Tyrannosaurus/  (name must match the species list in the generator exactly)
    model.obj
    model.mtl
    mask.png           required
    detail.png          optional
```

## `model.obj` + `model.mtl`

Standard OBJ/MTL export. One shared UV space for the whole model — the mask
below is applied as the diffuse map on *every* material found in the file, so
don't split the model across multiple non-overlapping UV tiles expecting
different masks per material.

## `mask.png` — the ID/segment map

Same UV layout as the model. Paint each region with one of these exact flat
colors to mark which of the six skin-code segments controls it. At runtime
every texel is matched to the nearest of these six colors and replaced with
the live segment color, so keep regions solid (no gradients/anti-aliasing) —
soft edges will get pulled toward whichever key color they're closer to.

| Segment | Key color (paint this) |
|---|---|
| Pattern Prefix | `#FF0000` |
| Underbelly | `#00FF00` |
| Body | `#0000FF` |
| Flank | `#FFFF00` |
| Markings | `#FF00FF` |
| Male Display | `#00FFFF` |

Resolution: 512×512 or 1024×1024 is plenty — it's recolored per-pixel on the
CPU on every color change, so bigger textures mean slower live updates.

## `detail.png` — optional shading

Grayscale (or just desaturated) texture, same UV layout. If present, it's
multiplied against the recolored mask output for subtle AO/shading. If
omitted, regions render as flat color.
