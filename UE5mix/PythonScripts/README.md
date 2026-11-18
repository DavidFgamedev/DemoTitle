# Unreal Engine 5 editor automation

Python that runs **inside the Unreal Editor**, against the `unreal` module exposed by the
Editor Scripting Utilities plugin. None of it is gameplay code and none of it ships in a
build. These are authoring tools: they create assets, wire material graphs and populate
levels, doing by script what would otherwise be done by hand in the editor UI.

Run them from the editor's Python console, or via **Tools > Execute Python Script**.

## The scripts

### `masterMaterial.py`
Builds a standard PBR master material from nothing. Creates `M_Master` in
`/Game/MasterMaterials` via `AssetToolsHelpers` and `MaterialFactoryNew`, then uses
`MaterialEditingLibrary` to add a `TextureSampleParameter2D` per surface channel and
connect each to its material property: base colour, roughness, normal, ambient occlusion
and metallic, with a `MaterialExpressionConstant` driving specular. The point is that the
graph is assembled in code, so every material in a project starts identical instead of
being rebuilt by hand per asset.

### `masterMaterialStacked.py`
The same master material built for **ORM-packed textures**, where occlusion, roughness and
metallic share one image across three channels. Creates `M_Master_Stacked`, drops the
separate roughness sampler the unpacked version needs, and additionally creates a
`MaterialInstanceConstant` (`masterMatStacked_ORM_Inst`) parented to it. Kept alongside
the unpacked version rather than replacing it, because which one a project wants depends
on how its textures were authored.

### `MeshPaintMaterialDemo.py`
The largest of these, and a vertex-colour blend material for mesh painting. Creates
`M_MeshPaint`, then builds a blend network from **5 `MaterialExpressionVertexColor`**
nodes, **5 `MaterialExpressionLinearInterpolate`** nodes and **5 `MaterialExpressionOneMinus`**
nodes, with texture sample parameters for base colour, normal and ORM. Each surface is
lerped against the next using a vertex colour channel as the alpha, so an artist can paint
between material layers directly on the mesh in the editor. Finishes by creating
`MeshPaintInstance` and parenting it with `set_material_instance_parent`, so the painted
result is edited through instance parameters rather than by touching the graph.

### `addAtmosphere.py`
Populates an empty level with a complete sky and lighting setup in one pass:
`DirectionalLight`, `SkyLight`, `SkyAtmosphere`, `ExponentialHeightFog` and
`VolumetricCloud`, each spawned at the origin through
`EditorLevelLibrary.spawn_actor_from_class`, then saves the level via the
`LevelEditorSubsystem`. Replaces five manual drag-and-drop steps that are identical every
time and easy to do inconsistently.

### `newLevelAndSave.py`
Creates a level at `/Game/Levels/newLevel` with `LevelEditorSubsystem.new_level`, sets it
as the current level, and saves it. The smallest script here and the building block the
others assume: a known-good empty level, created the same way every time.

## APIs these work against

`unreal.AssetToolsHelpers` · `unreal.MaterialEditingLibrary` · `unreal.MaterialFactoryNew` ·
`unreal.MaterialInstanceConstantFactoryNew` · `unreal.MaterialExpression*` ·
`unreal.MaterialProperty` · `unreal.EditorAssetLibrary` · `unreal.EditorLevelLibrary` ·
`unreal.LevelEditorSubsystem`

## A note on paths

Asset paths are hardcoded to `/Game/MasterMaterials` and `/Game/Levels`, and asset names
are fixed. They are written as demonstrations of the API rather than as a configurable
tool, so change the string literals at the top of a script before running it in a project
that already uses those paths.
