# Borrowed Torch Downloads

Tools Borrowed Torch made for its own pipeline, free to use in yours. This repository holds their releases and their
guides; their source is kept elsewhere. The newest version of each is also on
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/).

| Tool | What it does | Runs on | |
|------|--------------|---------|-|
| **Kindling Engine Editor** | The editor of Kindling, Borrowed Torch's game engine: a scene lit, ray-traced and shown as the engine draws it, saved as a project and played in a player of its own - with the game's own C# code run as you edit it, editors for its shaders, surfaces and game UI, and game types to start from. | 64-bit Windows, .NET 10 Desktop Runtime (the .NET 10 SDK for a game's code) | [Guide](KindlingEngineEditor/README.md) · [Releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=kindling-engine-editor) |
| **ORMH Converter** | Packs the ambient occlusion, roughness, metallic and height maps of a PBR material into one RGBA texture and saves it as a PNG. Packs an albedo with its opacity, too. | 64-bit Windows, nothing to install | [Guide](OrmhConverter/README.md) · [Releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=ormh-converter) |
| **Localization Helper** | Keeps a game's translations in order: its languages, the folders and JSON files the texts are split into, the string IDs, and the text of every ID in every language, with the reference language beside the one being written. | 64-bit Windows, nothing to install | [Guide](LocalizationHelper/README.md) · [Releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=localization-helper) |

Each tool's releases are tagged with its name: `kindling-engine-editor-v1.1.0`, `ormh-converter-v1.0.1`,
`localization-helper-v1.1.0`.

The Kindling Engine Editor's releases also carry the packages a game made with it builds against, at the editor's
version - `BorrowedTorch.Engine`, `BorrowedTorch.Editor` (the editor SDK) and `BorrowedTorch.Templates` (its game types,
for `dotnet new`) - which are on [nuget.org](https://www.nuget.org/profiles/BorrowedTorchStudio) as well.

## Checking a download

Every release carries a `.sha256` file next to its zip. To check a download is the file that was released, compare
that with:

```
Get-FileHash <the zip> -Algorithm SHA256
```

None of the tools is code-signed yet, so Windows SmartScreen may stop one the first time it runs with **Windows
protected your PC**. Choose **More info**, then **Run anyway**.

## License

MIT, for this repository and every tool in it alike - see [LICENSE](LICENSE). Each tool's license is also in its zip.
