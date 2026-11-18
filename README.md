# DemoTitle

**A code-sample repository, not a game project.** It holds loose source files from Unreal
Engine 5 learning work and editor tooling, uploaded as reference. There is no `.uproject`
here, no content, and nothing that builds. If you are looking for a shipped game, this is
not it, and the last section says where to find one.

Nothing here is deleted or hidden. This README exists because the directory names imply
more than the files contain, and that is worth stating up front rather than leaving a
reader to work out.

## What is actually in here

### `UE5mix/PythonScripts/`, the part worth reading
Python that runs inside the Unreal Editor to generate materials, build vertex-colour blend
graphs, and populate and save levels. This is real tooling and the strongest material in
the repository. **See [its own README](UE5mix/PythonScripts/README.md)** for what each
script does and which `unreal` APIs it drives.

### `UE5mix/C++Blueprints/`, coursework
Standalone exercise classes from the Vertex School programme and related practice:
`floatingActor`, `lightSwitch` and `lightSwitchBP`, `explodeBarrel`, `ProjectileDemo`, plus
`ChatGPTAPIWrapper`, and the `DevFlow` project's template character and game mode. These
are individual learning exercises, each demonstrating one engine concept. They are not
parts of a game and do not combine into one.

### `DemoTitle/Code/CharacterCode/ThirdPerson/Chale/`, a renamed template
**This is Epic's stock third-person template with the class renamed, and nothing more.**
`ChalesTributeCharacter.cpp` is 129 lines, carries Epic's own
`// Copyright Epic Games, Inc. All Rights Reserved.` header, and defines exactly the
template's five members: the constructor, `BeginPlay`, `SetupPlayerInputComponent`, `Move`
and `Look`. There is no combat code in it: no combos, no counters, no finishers, no dash,
no projectiles, no target lock. The identical 129-line file appears again in this
repository as `DevFlowCharacter.cpp`, which is the same template under a different project
name.

The `Chale` directory name refers to Chale's Tribute, but **this code is not that game.**
It is the starting template a project begins from, before any of the work happens.

### `DemoTitle/Blueprints/` and `Implementation/`
Three screenshots of animation blueprint graphs, and a three-line notes file. Reference
images, not source.

## Where the actual work is

Chale's Tribute is a third-person hack and slash built solo in Unreal Engine 5, in C++ and
Blueprints. Its combat systems, behavior-tree companion AI and encounter scripting are not
in this repository, because the game's source is not public. The artifacts that do show
the work:

- **Playable build:** https://davidfgamedev.itch.io/chales-tribute
- **Combat encounter design case study:** https://davidfgamedev.vercel.app/work/chales-tribute
- **Project breakdown:** https://davidfgamedev.artstation.com/projects/XJvJRY

Judge the game by the build and the case study. Judge this repository by the Python
tooling, which is the part of it that is genuinely mine and genuinely useful.
