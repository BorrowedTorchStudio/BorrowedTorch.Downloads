# Kindling Engine Editor

The editor of Kindling, Borrowed Torch's in-house game engine: a WPF editor around a MonoGame viewport (DirectX 11,
with compute), where a scene is lit, traced and shown as the engine draws it.

It is an early build. **Scenes are not saved yet**: the editor opens on a demo scene, and what is built in it lasts
until the editor is closed.

What you get:

- **A traced viewport**: PBR materials, with traced shadows and ambient occlusion on the GPU; see-through materials,
  cutouts whose holes the shadows see through, emissive surfaces with bloom, a choice of tone mappers, and
  auto-exposure.
- **Entities**: a viewer and an inspector, a transform gizmo, and lights - directional, point and spot - with handles
  for their reach.
- **Cameras**: camera entities to look through or open in a floating preview, camera paths with waypoints, and the
  camera modes they move in.
- **A shader editor**: the engine's own shaders, recompiled into the running viewport as they are edited.
- **A UI markup editor**: the game UI as markup, shown in the viewport as you type.
- **Audio emitters**: sounds placed in the scene, with their effect chains.

## Download

Get `BorrowedTorch.EngineEditor-<version>-win-x64.zip` from
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/#kindling-engine-editor) - or any version from
[this repo's releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=kindling-engine-editor) -
unzip it anywhere and run `BorrowedTorch.EngineEditor.exe`. Nothing is installed by it - deleting the folder removes it,
but for the caches below.

The exe is not code-signed, so the first time it runs, Windows SmartScreen may stop it with **Windows protected your
PC**. Choose **More info**, then **Run anyway**.

Each release also carries a `.sha256` file. To check a download is the file that was released, compare it with:

```
Get-FileHash BorrowedTorch.EngineEditor-<version>-win-x64.zip -Algorithm SHA256
```

## Requirements

- 64-bit Windows, and a graphics card with DirectX 11 compute shaders.
- The [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0). Without it the exe says so, and
  links to the download. The editor runs two .NET tools besides itself - the shader compiler and the texture importer -
  which is why it uses the installed runtime rather than carrying one of its own.

## First run

The engine compiles its shaders when it starts. The first start takes a few seconds longer for it; after that, a
shader whose source has not changed is read back from the cache. Compiled shaders and imported textures are kept under
`%LOCALAPPDATA%\BorrowedTorchEngine` - delete that folder to clear them.

## Controls

In the viewport:

| Input                     | Does                                                                    |
|---------------------------|-------------------------------------------------------------------------|
| Middle mouse button, drag | Look around - or orbit, in the orbiting camera mode                     |
| `W` `A` `S` `D`           | Fly forward, left, back, right                                          |
| `C`, `Z`                  | Fly up, down                                                            |
| `Q`, `E`                  | Roll                                                                    |
| `Left Shift`, held        | Fly slowly                                                              |
| Left click                | Select what is under the cursor                                         |
| `Esc`                     | Clear the selection                                                     |
| `R`                       | Cycle the gizmo: move, rotate, scale                                    |
| `G`                       | Gizmo axes: world or the selection's own                                |
| `1` `2` `3`               | Camera mode: free, chase, orbiting chase                                |
| `4`                       | Play a camera path                                                      |
| `P`                       | Add a waypoint where the camera is: after the selected one, or anew     |
| `O`                       | Show or hide the camera paths                                           |

The toolbar holds the rest: the panes, the cameras, and how the scene is shown - wireframe, ambient light, bloom, tone
mapping and exposure.

## License

MIT.
