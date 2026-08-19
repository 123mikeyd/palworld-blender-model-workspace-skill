---
name: palworld-blender-model-workspace
description: Prepare personal Palworld models for Blender painting.
version: 0.1.0
author: Mike (123mikeyd), Hermes Agent
license: MIT
platforms: [windows]
metadata:
  hermes:
    tags: [palworld, blender, ueformat, fmodel, texture-paint]
    related_skills: [palworld-asset-pipeline, palschema-modding]
---

# Palworld Blender Model Workspace

Use this skill to help a player extract their own Palworld character assets, import Blender-ready `.uemodel` files with UEFormat, connect textures, and create a simple texture-paint workspace. It is for personal modding/reference work; it does not distribute game assets.

## When to Use

- A user wants their own Palworld model in Blender for painting or mesh edits.
- A user needs the UEFormat Blender add-on installed or verified.
- A user wants a compact model gallery with readable textures.
- Do not use this for UE5.1 cooking, pak construction, or PalSchema data changes; load the appropriate Palworld asset/data skill after the art edit is approved.

## Prerequisites

- The user owns Palworld and extracts only their own installed-game assets.
- Blender 4.2 or newer.
- Current UEFormat Blender add-on from the official h4lfheart/UEFormat project.
- FModel (or an equivalent extractor) configured for the user’s installed Palworld build.
- One self-contained folder per Pal containing the `.uemodel`, extracted base-color textures (`*_B.png`), normal/packed maps, and material JSON when available.

Never upload, publish, or share extracted game assets. Sharing this skill is fine; every friend must perform their own extraction.

## Procedure

1. **Create only the needed workspace folders.** Keep a `Player` folder and add Pal body-family folders only when a Pal is actually imported. Completion: the user can locate a Pal by name without browsing a dump folder.
2. **Install UEFormat.** In Blender Preferences, install/enable the UEFormat `.uemodel` add-on. Verify Blender exposes its UE Format import panel and recognizes `.uemodel` as an importable format.
3. **Extract a complete Pal set.** Use FModel to export the skeletal mesh plus its required texture/material companions into one Pal folder. Completion: the folder contains a `.uemodel` and at least the relevant base-color texture(s), not only `.uasset/.uexp` packages.
4. **Import the `.uemodel`, not raw Unreal package files.** Import at LOD 0 with skeleton, weights, and sockets preserved. Do not use `.uasset` or `.uexp` as a Blender starting point.
5. **Connect preview textures.** Connect `_B` as sRGB Base Color. Use Non-Color for `_N` through a Normal Map node; treat packed `_M` maps as game-reference data until their channels are verified. Completion: Material Preview shows the Pal's actual body colors instead of solid white or magenta.
6. **Make a paint-safe body texture.** Duplicate the body base-color image before painting and assign the duplicate to the body material. Pack images into the `.blend` if the user needs a portable single-file workspace. Completion: painting cannot overwrite the extracted original.
7. **Use one active Pal at a time.** In Object Mode select the intended mesh, then enter Texture Paint. Keep any comparison models separate from exports. Completion: the active material's image is visibly selected as the paint target.
8. **Preserve animation compatibility.** Do not rename bones, alter the armature hierarchy/rest pose, or apply transforms to the imported armature during casual art edits. Completion: mesh edits are saved in a named working `.blend`, leaving the source model intact.
9. **Hand off deliberately.** When the user says the art edit is done, verify the texture file, Blender scene, and an FBX round trip before starting UE5.1 cooking/pak work.

## Pitfalls

- White models can be a viewport-shading issue: switch the 3D viewport to Material Preview before assuming textures are absent.
- Magenta means Blender cannot find an image; use packed images or repair the image path.
- A `.uasset/.uexp/.ubulk` dump is not a ready Blender import. Export a `.uemodel` first.
- A Blender viewport preview is not proof that a Palworld pak will work. UE5.1 and in-game testing remain separate gates.
- Do not delete sources merely because a packed `.blend` opens. Keep a Git-backed source archive for later mod packaging.

## Verification

- Open the saved `.blend` in a new Blender process and confirm all intended body materials display in Material Preview.
- Confirm the active texture-paint image is a working copy, not the extracted original.
- Confirm the Pal still has its original armature, bone names, material slots, and vertex groups.
- Back up the workspace before changes; Git status should be clean after a successful backup.
