# Borrowed Torch Downloads

Tools Borrowed Torch made for its own pipeline, free to use in yours. This repository holds their releases and their
guides; their source is kept elsewhere. The newest version of each is also on
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/).

| Tool | What it does | Runs on | |
|------|--------------|---------|-|
| **ORMH Converter** | Packs the ambient occlusion, roughness, metallic and height maps of a PBR material into one RGBA texture and saves it as a PNG. | 64-bit Windows, nothing to install | [Guide](OrmhConverter/README.md) · [Releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=ormh-converter) |

Each tool's releases are tagged with its name: `ormh-converter-v1.0.1`.

## Checking a download

Every release carries a `.sha256` file next to its zip. To check a download is the file that was released, compare
that with:

```
Get-FileHash <the zip> -Algorithm SHA256
```

None of the tools is code-signed yet, so Windows SmartScreen may stop one the first time it runs with **Windows
protected your PC**. Choose **More info**, then **Run anyway**.

## License

MIT, for this repository and the ORMH Converter alike - see [LICENSE](LICENSE). Each tool's license is also in its zip.
