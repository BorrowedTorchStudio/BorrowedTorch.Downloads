# ORMH Converter

A small Windows app that packs the four single-channel maps of a PBR material into one ORMH texture - **R** = ambient
occlusion, **G** = roughness, **B** = metallic, **A** = height - and saves it as a PNG. For texture packs that ship
those maps separately, and for projects that want packed PNGs rather than packing in a content build.

It packs with [BorrowedTorch.Ormh](https://www.nuget.org/packages/BorrowedTorch.Ormh), the same code
[BorrowedTorch.ContentPipeline](https://www.nuget.org/packages/BorrowedTorch.ContentPipeline) builds with, so a map
packed here is byte for byte the map a content build makes from the same sources.

What you get:

- **A window**: one card per channel, each an image (any of its channels, optionally inverted) or a constant, with
  a preview of every channel and of the packed result.
- **Drop to sort**: drop a texture pack's maps anywhere on the window and they go to their channels by name.
- **Descriptors**: open or drop an `.ormh` descriptor to fill all four channels - the same file `Content.mgcb` builds
  from.
- **A command line**: `BorrowedTorch.OrmhConverter <descriptor.ormh> <output.png>`, for batches and scripts.

## Download

Get `BorrowedTorch.OrmhConverter-<version>-win-x64.zip` from
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/#ormh-converter) - or any version from
[this repo's releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=ormh-converter) -
unzip it anywhere and run `BorrowedTorch.OrmhConverter.exe`. It is one self-contained file: nothing has to be installed first, .NET
included, and nothing is installed by it either - deleting the folder removes it.

The exe is not code-signed, so the first time it runs, Windows SmartScreen may stop it with **Windows protected your
PC**. Choose **More info**, then **Run anyway**.

Each release also carries a `.sha256` file. To check a download is the file that was released, compare it with:

```
Get-FileHash BorrowedTorch.OrmhConverter-<version>-win-x64.zip -Algorithm SHA256
```

## Requirements

- 64-bit Windows. Nothing else.

## The window

### Channels

| Card                      | Constant it starts at | Which means                         |
|---------------------------|-----------------------|-------------------------------------|
| **R** · Ambient occlusion | 1                     | no occlusion                        |
| **G** · Roughness         | 1                     | fully rough                         |
| **B** · Metallic          | 0                     | not metallic                        |
| **A** · Height            | 1                     | flat, at the top of the heightfield |

Each card is either an **Image** or a **Constant**:

- **Image**: browse for one, or drop it onto the card. **Read** picks which of the image's channels to take - R by
  default, which is right for a map saved as grey. **Invert** stores 255 minus the value, for a gloss or smoothness
  map read as roughness.
- **Constant**: one value from 0 to 1 over the whole map, set with the slider or typed in.

### Dropping files

Files dropped on the window are sorted into channels by name, ignoring case:

| The name contains                            | It goes to          |
|----------------------------------------------|---------------------|
| `rough`                                      | Roughness           |
| `gloss`, `smooth`                            | Roughness, inverted |
| `metal`                                      | Metallic            |
| `height`, `disp`, `parallax`                 | Height              |
| `occlusion`, or `ao` as a word or at the end | Ambient occlusion   |

A file it cannot place - an albedo or a normal map, say - is listed under **Output**; drop it onto a card instead.
One image dropped onto a card goes to that card whatever its name. An `.ormh` descriptor dropped anywhere, or opened
with **Open .ormh…**, fills all four channels.

### Output

The largest source sets the size of the map. A smaller one is stretched up to it the way a GPU magnifies it -
bilinear, wrapping at the edges - and **Output** says when that happens; a map of nothing but constants is a single
texel. The two previews show the packed RGB (occlusion, roughness, metallic) and its alpha (height) separately, since
alpha is not transparency here.

**Save PNG…** suggests a name from what the sources share - `Rock_AO.png` and `Rock_Roughness.png` give
`Rock_ORMH.png` - in the folder of the first one.

## The command line

```
BorrowedTorch.OrmhConverter <descriptor.ormh> <output.png>
```

Packs a descriptor without opening a window, and prints what it did to the console it was started from.

| Exit code | Meaning                                                                        |
|-----------|--------------------------------------------------------------------------------|
| 0         | Packed.                                                                        |
| 1         | The descriptor or one of its images could not be read; the message says which. |
| 2         | Wrong arguments; the usage line is printed.                                    |

It is a Windows app, so neither cmd nor PowerShell waits for it on its own. In a script, wait for it - and get its
exit code - with `start /wait` in cmd, or `Start-Process -Wait -PassThru` in PowerShell.

## The descriptor

A JSON file naming where each channel comes from, with paths relative to the descriptor:

```jsonc
{
    "ao": "Rock_AO.png",
    "roughness": { "source": "Rock_Gloss.png", "invert": true },
    "metallic": 0,
    "height": { "source": "Rock_Height.png", "channel": "r" }
}
```

| Form                                                      | Meaning                                                  |
|-----------------------------------------------------------|----------------------------------------------------------|
| `"Map.png"`                                               | The image's red channel.                                 |
| `{ "source": "Map.png", "channel": "g", "invert": true }` | Any of `r`, `g`, `b`, `a`; `invert` stores 255 minus it. |
| `0.5`                                                     | A constant from 0 to 1 over the whole map.               |

All four channels are required. Comments and trailing commas are allowed. The full format is described in
[BorrowedTorch.Ormh](https://www.nuget.org/packages/BorrowedTorch.Ormh).

## What goes into the PNG

- **8-bit RGBA, straight alpha.** Alpha holds height, not transparency: build the PNG with premultiplied alpha and
  colour keying off - in `Content.mgcb`, `PremultiplyAlpha=False` and `ColorKeyEnabled=False` - or the other three
  channels get scaled by height.
- **Values exactly as read.** Colour profiles are ignored: these maps are data, not colours.
- **16-bit sources** - grey, RGB or RGBA - are narrowed to 8 bits by the same rule MonoGame's importer applies to
  16-bit RGB, so such a map packs here exactly as it would in a content build. 16-bit grey is one MonoGame 3.8.3
  cannot read at all, in a content build or anywhere else; packing it here first is the way around that.
- **Floating-point images** are not read; export the map as an 8- or 16-bit PNG.
- **Formats**: PNG, JPEG, BMP and TIFF.

## License

MIT.
