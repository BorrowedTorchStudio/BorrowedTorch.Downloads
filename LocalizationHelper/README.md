# Localization Helper

A Windows app that keeps a game's translations in order: the languages, the folders and files the translated texts are
split into, the string IDs, and the text of every ID in every language. The texts are plain JSON files, one set per
language, so the game reads them without the app and they can be edited, diffed and merged like any other file.

What you get:

- **One folder for everything**: three small manifests and a folder per language, each laid out the same way.
- **A files structure**: one tree of folders and translation files, mirrored into every language's folder - add,
  rename, move or remove an entry once and every language follows.
- **String IDs**: each belongs to one file. Adding, renaming, moving or removing one updates that file in every
  language.
- **Translating side by side**: the text of the reference language above the one being written, and a mark on every
  language and string ID that still lacks a text.

## Download

Get `BorrowedTorch.LocalizationHelper-<version>-win-x64.zip` from
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/#localization-helper) - or any version from
[this repo's releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=localization-helper) -
unzip it anywhere and run `BorrowedTorch.LocalizationHelper.exe`. It is one self-contained file: nothing has to be
installed first, .NET included, and nothing is installed by it either - deleting the folder removes it. Its first start
takes a moment longer, while it unpacks the libraries it draws with into `%TEMP%\.net`.

The exe is not code-signed, so the first time it runs, Windows SmartScreen may stop it with **Windows protected your
PC**. Choose **More info**, then **Run anyway**.

Each release also carries a `.sha256` file. To check a download is the file that was released, compare it with:

```
Get-FileHash BorrowedTorch.LocalizationHelper-<version>-win-x64.zip -Algorithm SHA256
```

## Requirements

- 64-bit Windows. Nothing else.

## The localization folder

Everything the app writes lives in one main folder:

```
<main folder>/
  Languages.manifest        the languages: local name, English name, which one is the reference
  FilesStructure.manifest   the tree of folders and translation files
  StringIDs.manifest        the string IDs, and the file each one belongs to
  English/                  one folder per language, named after its English name,
    Menus/                  each holding the whole files structure
      main.json
  Ukrainian/
    Menus/
      main.json
```

A translation file is a JSON object from string ID to text:

```json
{
  "menu_start": "Почати гру",
  "menu_quit": ""
}
```

An empty text means "not translated yet". Every language's copy of a file holds every string ID of that file, so a
missing translation is an empty text rather than a missing key.

## The window

The top bar holds the main folder, the bottom bar reports what the last action did - in red, with its cause, when it
failed - and the three lists in between are filled once a folder is open. Every list has a search box that filters it
as you type.

### Main folder

**Select main localization folder** opens a folder:

- One that holds `Languages.manifest` is loaded.
- Any other is offered to be initialized as a new localization folder: the manifests are written into it, starting
  with no languages and an empty files structure. An empty folder is the best place to start one.

**Open main localization folder in explorer** shows it in Explorer, and **Save all** writes the text being edited and
the manifests at once.

### Languages

**Add language** asks for a **Local name** - the language's name in itself, `Українська` - and an **English name**,
`Ukrainian`, which also names its folder. The new language gets a copy of the whole files structure, every text empty.
**Edit language** renames it, folder included; **Remove language** deletes its folder with all its texts.

One language is the **reference**: the one the others are translated from, whose text is shown above the editor. The
first language added becomes it; **Mark language as reference** makes the selected one the reference instead.

The mark next to a language shows whether every string ID has a text in it.

### Files structure

The tree under **`<Root>`** is the layout every language folder gets. **Add entry** adds a folder or a file - tick
**Is folder** for a folder - inside the selected folder, in every language at once. Give a file the extension the game
expects, `main.json`. **Edit entry** renames the selected entry and **Remove entry** deletes it, with everything in it
and the string IDs of its files, in every language.

Drag an entry onto a folder to move it there, with its content.

Selecting a folder or a file narrows the string ID list to what it holds; selecting `<Root>` shows them all.

### String IDs

**Add string ID** asks for the ID and its **Location** - the file it belongs to, the selected one to start with - and
adds it, with an empty text, to that file in every language. An ID is unique across the whole folder, not only within
its file. **Edit string ID** renames it or moves it to another file, keeping its texts; **Remove string ID** removes it
from every language.

**Find location in files structure** selects the file the selected ID belongs to.

The mark next to a string ID shows whether it has a text in the selected language.

### Translating

Select a language and a string ID, then type in the text box. The reference language's text is shown above it. The
text is written to the language's file when another language or ID is selected, when **Save all** is pressed, and when
the app is closed.

## What it does on disk

- **Every change is written at once**, to the manifests and to the files of every language. A file is written whole to
  a temporary file first and then put in place, so a crash cannot leave a half-written one behind. Files are written as
  indented JSON.
- **Names are file names.** A language's English name and every folder and file name must be valid on Windows, and
  two names that differ only in letter case count as the same, as they do on disk.
- **A failed write is undone.** When a change cannot be written - a file open in another program, say - the bottom
  bar says why, and the change is taken back, so the lists, the manifests and the files keep agreeing with each other.
- **Opening a folder repairs it.** Language folders, translation files and string IDs that a file lacks are created
  again, empty, and the bottom bar says how many. A string ID whose file is no longer in the files structure is
  dropped, with a warning naming it.
- **The manifests hold only what cannot be worked out.** Which texts are missing is read from the translation files
  each time, so the files can be edited outside the app - by a translator, or by a merge - and the app shows the result
  the next time the folder is opened.

## License

MIT.
