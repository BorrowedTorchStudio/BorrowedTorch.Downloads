# Kindling Engine Editor

The editor of Kindling, Borrowed Torch's in-house game engine: a WPF editor around a MonoGame viewport (DirectX 11,
with compute), where a scene is lit, traced and shown as the engine draws it.

It is an early build. The editor opens on a demo scene; the File menu saves what is built as a **project** - its
scenes and its assets in a folder of its own - and opens it again (see [Projects](#projects)). The **player** beside it
plays a project as the game will be, in a window of its own (see [Player](#player)), and a project's own C# code adds
to the engine what the game needs (see [Game code](#game-code)).

What you get:

- **A traced viewport**: PBR materials, with traced shadows and ambient occlusion on the GPU; see-through materials -
  glass and water bending what is seen through them, frosted where they are rough, sharp where something touches them
  and blurred the farther behind it is - cutouts whose holes the shadows see through, emissive surfaces with bloom, a choice of tone mappers,
  auto-exposure, and temporal anti-aliasing - or NVIDIA DLSS, where it is installed (below) - drawn at the viewport's
  own size or smaller and upscaled to it, for a faster frame; with the frame rate in its corner, and, from the Debug
  menu, how long the GPU spends on each pass.
- **Entities**: a viewer and an inspector - an entity made unpickable there clicked through in the viewport, to what is
  behind it, several picked at once by a frame dragged about them there, as units are in a strategy game -
  a transform gizmo - snapped with the toolbar's **Snap**: moves on the grid, drawn at the step, turns in steps of an
  angle, scales in tenths - and lights - directional, point and spot - with handles for their reach; a point or
  spot light can glow in the air about it, a halo about a lamp or a spot's cone seen, shadowed by what stands in its
  light - dark before a wall, in shafts through a grate's gaps, tinted through colored glass - and seen through glass
  as the rest of the scene is: dimmed, bent by a lens, tinted by thick glass.
- **Cameras**: camera entities to look through or open in a floating preview, camera paths with waypoints, and the
  camera modes they move in. One is ticked as the **Game camera**, the one the game is seen through in the player; a
  camera can fly its path as the game starts. A camera is perspective, through its **Field of view**, or
  **Orthographic**: it looks along parallel lines, its **Height** in world units from the bottom of the view to the top
  at every depth, so what is further off keeps its size - for a 2D game looking down -Z at sprites on the XY plane. All
  of the lighting holds through it - shadows, occlusion, bounced light, reflections, glass, fog and the lights' glow -
  and the sky is drawn behind the scene as through a lens of the camera's own field of view, a backdrop, clouds and
  all. Keep an orthographic camera near what it shows: the fog's and the glow's grid runs some 80 m out from its plane.
  A sound is heard through it where it shows on screen, as near as the camera's lens would frame the view - however
  far back the camera stands.
  The viewport's own camera has a **2D** view, on the toolbar: orthographic, looking down -Z at the XY plane with the
  grid stood up on it, panned and zoomed but never turned - the 3D view comes back as it was when it is turned off. A
  camera entity moved to the view from it is made orthographic too, as tall, ready for a 2D game. Looking through a
  camera entity, the wheel, the middle-drag and the fly keys move that camera as they move the viewport's own - an
  orthographic one zoomed by its Height and panned, never turned - its pose and Height kept with the scene; one that a
  chase or its path moves is left to them, an orthographic one only zoomed.
- **Animations**: every property of an entity that can be animated - a transform, a light's color and intensity, the
  sky's time and clouds, the wind, the fog, a cloth's stiffness - has an A at the end of its row: grey without an
  animation, red stopped, yellow paused, green running. A click opens its keys, times and values sorted by time; it goes
  from one to the next at once or blended, and plays once, loops, ping-pongs, or accumulates a step every period for as
  long as it plays - five degrees a second, say. The A's menu starts, pauses and stops it. They all play from their
  starts as the game plays, pause with it, and give their properties back their own values as it stops; one started by
  hand plays while the scene is edited.
- **A shader editor**: the engine's own shaders, recompiled into the running viewport as they are edited.
- **A UI markup editor**: the game UI as markup, shown in the viewport as you type - one markup of the project the
  game's UI, which the player shows over the game.
- **Audio emitters**: sounds placed in the scene, with their effect chains - played as the game starts, where an
  entity says so.
- **Wind**: one for the scene, blowing the way its entity faces, in metres a second - slower near the ground, the
  rougher the ground the more - and swinging about that in gusts, eddies a metre or so across in it; the grass pushed
  over by it against each kind's stiffness, a patch at a time, cloth billowing in it, the procedural sky's clouds
  drifting on it, their shadows with them - on the scene's own clock, which runs while the game plays.
- **Fog**: one for the scene, lying over it at a density and a height, thinning above it, broken into wisps that drift
  on the wind - and fog volumes, boxes of dust or smoke wherever they are placed; what lies behind them dimmed and
  greyed toward their color, lit by the sky; every sun shining through them in sharp shafts, under the clouds' shadow,
  and the point and spot lights in them glowing, each as much as its Glow, shadowed as the air is.
- **Vegetation**: fields of grass, wheat, flowers - or kinds of your own, with their own shape, stiffness, colors, glow
  against the sun and textures - growing on the meshes marked as their ground, swaying in the wind, in the traced light and
  shadows, dark down among their blades and shading the ground between them, which takes on their color; where each
  kind grows painted with a brush, and trampled by whatever is made a trampler. A field is grown and culled by cell, so a
  large one costs only the part of it in view, and a brush stroke grows only what it painted again; grass hidden behind
  walls and hills is never drawn.
- **Fabric**: sheets of cloth hung where their entities stand and pinned where you paint them, simulated on the GPU -
  blowing and rippling in the scene's wind and its eddies, a flag flapping even in steady air, falling in folds over the
  scene's meshes and against its walls, drawn on both sides in any material, glass and blended ones seen through -
  casting the traced shadows, and found by the bounced light and the reflections where they are, the light about them
  keeping up as they move. Cut and ripped by hand in the viewport, as you see them, and torn where a gale pulls them
  too hard.
- **Colliders**: boxes, spheres, capsules and cylinders an entity collides as - and rectangles, circles and capsules in
  the XY plane, for a side view - each a trigger or solid, on layers, with what the game's code asks of them: what a
  ray meets, what overlaps a volume, a body moved and slid along walls and floors, and what comes into a trigger and
  goes out of it while the game plays (see [Collisions](#collisions)). Their wires show on the selection, or all of them
  from the Debug menu; no rigid bodies - nothing moves by itself.
- **Frustum culling**: each view - the viewport, a camera preview, the player's - draws only the meshes it can see, each
  mesh's bounds kept in a tree that hands a view whole branches at once; the traced light still sees the whole scene,
  so a shadow or a reflection of something out of view stays. The Debug menu turns it off to compare, shows the bounds,
  and counts what was drawn under the GPU timings.

## Download

Get `BorrowedTorch.EngineEditor-<version>-win-x64.zip` from
[borrowedtorch.com/downloads](https://borrowedtorch.com/downloads/#kindling-engine-editor) - or any version from
[this repo's releases](https://github.com/BorrowedTorchStudio/BorrowedTorch.Downloads/releases?q=kindling-engine-editor) -
unzip it anywhere and run `BorrowedTorch.EngineEditor.exe`; `BorrowedTorch.Player.exe` beside it is the player. Nothing is installed by them -
deleting the folder removes them, but for the caches below.

The exes are not code-signed, so the first time each runs, Windows SmartScreen may stop it with **Windows protected
your PC**. Choose **More info**, then **Run anyway**.

Each release also carries a `.sha256` file. To check a download is the file that was released, compare it with:

```
Get-FileHash BorrowedTorch.EngineEditor-<version>-win-x64.zip -Algorithm SHA256
```

## Requirements

- 64-bit Windows, and a graphics card with DirectX 11 compute shaders.
- The [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0). Without it the exe says so, and
  links to the download. The editor runs two .NET tools besides itself - the shader compiler and the texture importer -
  which is why it uses the installed runtime rather than carrying one of its own.
- The [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0), only to build a game's own code (see
  [Game code](#game-code)).

## NVIDIA DLSS (optional)

On an NVIDIA RTX card, NVIDIA DLSS can stand in for the engine's anti-aliasing and upscale the viewport. It is not in
the release: it is a module of two DLLs beside the exe - a small bridge built against NVIDIA's
[DLSS SDK](https://github.com/NVIDIA/DLSS) and the SDK's own `nvngx_dlss.dll` - under NVIDIA's RTX SDK license.
Without them the editor uses its own anti-aliasing and upscaling, and **Settings > NVIDIA DLSS** is greyed, its tooltip
saying why.

## First run

The engine compiles its shaders when it starts. The first start takes a few seconds longer for it; after that, a
shader whose source has not changed is read back from the cache. Compiled shaders and imported textures are kept under
`%LOCALAPPDATA%\BorrowedTorchEngine` - delete that folder to clear them. The render resolution and the DLSS switch are
remembered there too, in `Settings\editor.json`, with the projects opened last.

## Projects

A project is a folder: its project file, its scenes and its assets.

```
MyGame\
  MyGame.btproject        its name, the scene it opens with, the game's UI and its code
  Scenes\Main.btscene     every entity and its components, and how the scene is shown: background, bloom, tone
                          mapping, exposure
  Assets\
    Textures.btassets     the imported textures: name, file, import options
    Shaders.btassets      the shaders added in the shader editor or made in the surface graph
    Materials.btassets    every material, the built-in ones too, as edited
    CameraModes.btassets  the camera modes, Free aside
    Vegetation.btassets   the kinds of vegetation, and the painted masks (Masks\*.png)
    Audio.btassets        the mix and the emitters: their clips, loops and effect chains
    AudioGraph.btassets   where the audio panel's effects sit
    Textures\ Shaders\ Audio\ Masks\   the files those name, copied in
    Ui\*.btui             the UI markups - one of them the game's UI - and the style dictionaries they include
    Subgraphs\*.btsubgraph  the surface graph's subgraphs
  Code\                   the game's own code, a C# project (see Game code), its editor code another in Code\Editor;
                          both built into Code\bin
  .btuser                 the scene you had open, and where the camera was in each scene (yours alone: leave it out of
                          version control)
```

Every file is JSON, indented, the same bytes for the same scene - a change shows as a small diff. A file picked from
anywhere - an image or a packed map and the images it names, a clip, a shader and the headers beside it - is copied into
`Assets` as it is picked, while a project is open, and used from there on; one picked before there was a project is
copied in when the project is first saved. So the folder holds all it needs.

- **New project** starts a project from a game type (see Templates): an empty game, a game with code of its own, a
  first-person walker, a 2D platformer. It is made in a folder of its own named after it, and the editor starts again
  on it. **Save** with no project open saves what is open - the demo, say - as a new project instead.
- **Open project** (or Recent projects) starts the editor again on the project, so nothing of another project's assets
  stays in it; **Open scene** and **New scene** show another of its scenes, or an empty one, in place.
- **Save** (`Ctrl+S`) writes the assets and the scene; **Save scene as** another scene file in the project; **Save
  project as** copies the whole project to a new folder and goes on there.
- A scene opens where you last looked at it from: the camera's place, its mode, and the scene camera you looked through.
- The window's title is marked `*` while something is unsaved - compared with the last save shortly after each edit (a
  key or a click let go of, in the editor or the viewport), and not at all while you only look. Before anything would
  be lost - closing, opening another project or scene - the editor asks whether to save it. A game playing is stopped
  first: a scene is saved as it is edited, not as it plays.
- What the engine does not know - a feature it was built without - is kept as it was read and written back: an
  entity's components, a document in `Assets`. Anything worth knowing about an open or a save - that, a reference to an
  entity that is gone, a texture a material names that is not there - is listed in a message.

The editor starts on a project given on its command line: `BorrowedTorch.EngineEditor.exe MyGame\MyGame.btproject
[--scene Scenes\Other.btscene]`.

## Templates

**File > New project** starts a project from a game type. Pick one, name the project and pick the folder it goes in; the
editor makes the project in a folder of its own named after it, and starts again on it.

| Game type      | What it is                                                                                                                                                                                                           |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Empty game     | A sky with its sun, the ground and the game camera. No code: **File > New game code** adds it when it is wanted.                                                                                                     |
| Game with code | The empty game with C# code of its own (see Game code): a component that spins its entity while the game plays, and editor code - the component's view in the inspector, a Spinners panel, a **Game** menu command, a surface-graph node, an overlay drawing the spinners' axes. |
| Platformer 2D  | A side-view platformer seen through an orthographic game camera: `A` `D` or the arrows run, `Space`, `W` or up jump - with coyote time - over a crate and platforms to the flag. Sprites on the XY plane, the player moved by its rectangle collider through the Collisions feature's move and slide, the flag a trigger; the camera follows. The player's speeds and jump are in its inspector. |
| Walker         | A first-person walk among a few blocks: `W` `A` `S` `D` or the arrows walk the game camera, `Shift` runs, the right mouse button held turns the view. The walker's speeds are in its inspector.                      |

The game types with code build against the engine and editor SDK packages of the editor's version, so they take the
.NET 10 SDK, and NuGet the first time. The game's name in its code is the project's name, kept to letters and digits.

They are `dotnet new` templates too, in the `BorrowedTorch.Templates` package released with the editor:

```
dotnet new install BorrowedTorch.Templates
dotnet new kindling-walker -n MyGame
```

`kindling-empty`, `kindling-game`, `kindling-walker` and `kindling-platformer` make the same files as the editor does;
`--title` gives the name the editor shows, where it is not the project's own. The editor carries them in `Templates\`
beside it.

## Player

The player plays a project as the game will be: the engine in a window of its own, with nothing of the editor's - no
gizmos, no handles, no editor camera, no frame rate. **File > Play in player** (`F5`) saves the project, then plays the
scene open in it. Or from the command line:

```
BorrowedTorch.Player.exe MyGame\MyGame.btproject [--scene Scenes\Other.btscene]
```

With no project given, it plays the one in a `Game` folder beside the exe, or the one beside the exe itself - where a
game is shipped with its player. It opens the project's startup scene, or the one asked for, and plays it:

- **Through the game camera**: the camera entity ticked **Game camera** in its inspector - one at a time, ticking one
  takes the tick off the others. With none ticked, the first camera in the Cameras list; with no camera at all, the
  engine's own view.
- **With the game's UI** over it: the markup the project names - in the UI markup editor, **Use as the game's UI** names
  the markup edited, saving it into the project's `Assets\Ui` first. Its animations started on load start as the game
  does. A project with a UI opens it in the markup editor, so the viewport shows it as the game will.
- **Starting what is ticked to**: an audio entity's **Play on start** plays its emitter, a camera's **Play path on start**
  flies it along its path - in the player, and in the editor's Play too. A pause holds them; Stop stops them, the
  cameras put back where they stood.
- **Running the game's code** (see Game code), as it was built last - Play in player builds it first where it
  changed. Code that is not built, or that the engine does not start with, is left out, and a message says why.

`Esc` closes it and `Alt` + `Enter` goes to full screen, borderless, and back - once the game has had the key, and only
where it has not taken it: a key of the game's bound to `Esc` (its menu, say) keeps the player open, as does its code
saying so (`PlayerWindow.TakesEscape`, `TakesFullScreenKey`). A game that takes them closes the player and fills the
screen itself (`PlayerWindow.Close`, `SetFullScreen`); in the editor's Play these do nothing. Before the game plays, and
once its code has failed, `Esc` closes the player whatever the game said.

## Game code

A game's own code is a C# project in its project's `Code` folder, and its editor code another in `Code\Editor`: the
game types with code start with both, and **File > New game code** writes them into a project that has none - projects
building against the engine and editor SDK packages at the editor's version, and a feature to start from - builds them
and runs them.
Building it takes the [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0); the editor and the player
themselves need only the runtime.

The code is **features**, as the engine's own are: every public `IEngineFeature` of it, with a constructor taking
nothing, registers what it adds through its builder -

- **components** (`builder.Components.Add<T>()`): offered in Add component, saved with the scene, and edited in the
  inspector by their public fields - numbers, booleans, text, enums, vectors, colors, entities;
- **steps of every frame** (`builder.Update.Step`), in the stage they ask for - one that moves things while the game
  plays reads `frame.SceneClock.Elapsed`, which runs only then;
- **what happens as the game plays, pauses and stops** (`builder.Play.OnChanged`), **keys** (`builder.Input.Button`),
  **services**, and the rest the engine's features register;
- **handlers for the game's UI** (`builder.Ui.Handler("StartGame", ...)`): a markup's `OnClick="StartGame"` runs it while
  the game plays, in the player and in the editor's Play.

The editor builds the code as its files change, a second after the last change, and **runs each build in place** as
soon as it is built - well under a second - the editor running on: the scene, the camera, the selection and what is
unsaved stay as they are; the game's components are carried over to the new build's by their names, its asset sections
through their migrations. While the game plays, the build waits for Stop. The **Game code** panel says what runs, how
the last build and the last reload went - a build that does not go in leaves the one before running, and says why -
and how the code failed as the game played; its **Reload as it builds** switch turns this off, and **Reload** (or
**File > Reload game code**) then runs what is built. Its tab comes up for a failed build, a failure or a build that
did not go in. Code that throws as it runs is not run again until the game is played afresh, and the game is paused,
so a bug does not take the editor down. Stop puts the scene back as it was when Play was pressed - whatever the game's
code moved.

Each build is let go of once another runs, and goes from memory unless something still holds it. So a game's code
disposes what it starts - a thread, a timer, a handler on a static event - when its feature's services are disposed
(`IDisposable`, the last built first); the panel says when a build it let go of is still in memory. A native library
it loads stays loaded, and its file locked, until the editor closes. What it does as it goes in - registering,
building its services, setting up its content - runs on the engine's thread between two frames; an asset section that
saves files beside its document puts them in its data when the save is only carried to the next build
(`AssetContext.IsCarried`).

A game's code builds against `BorrowedTorch.Engine`, the engine as a NuGet package, and its editor code against
`BorrowedTorch.Editor` as well (see Extending the editor): both are on [nuget.org](https://www.nuget.org/packages/BorrowedTorch.Engine)
at each release's version, and attached to the release besides. A game's code asks for the version of the editor that
made it, and builds against it.

## Extending the editor

A game's editor code is a C# project of its own beside the game's, `Code\Editor\<Game>.Editor.csproj`, named in the
project file (`EditorCode`) - the Game with code template and **File > New game code** start with one. It builds against
`BorrowedTorch.Editor`, the editor SDK, released with the editor at its version: the contract the editor's own
features register through as well. The editor builds it with the game's code into `Code\bin`, loads it with each build
and lets go of it with that build; the player never loads it, so a game played there runs its engine features alone.

Its **editor features** register what they add to the editor. Every public `IEditorFeature` with a constructor taking
nothing is one, for the game's engine feature its `EngineFeatureId` names:

```csharp
public sealed class MyGameEditorFeature : IEditorFeature
{
    public string EngineFeatureId => MyGameFeature.FeatureId;

    public void Register(EditorFeatureBuilder builder)
    {
        // The component's own view in the inspector, in place of the generic one.
        builder.Inspectors.Add<SpinComponent, SpinView>(new ComponentDisplay("Spin", "🌀", Brushes.Orange),
            editor => new SpinView(editor),
            (view, spin, snapshot) => view.Show(spin, snapshot));

        // A document tab, opened from the View menu; a modal tool, shown by the code that needs it.
        builder.Panels.Add("Spinners", editor => new SpinnersPanel(editor));
        builder.Tools.Add(editor => new SpinSpeedTool(editor));

        // A command in a menu - the first part of its path is a top menu, made where there is none - and a switch in
        // the Debug menu.
        builder.Menus.Command("Game/Stop all spinners", StopAllSpinners, "Sets every spinner's speed to nothing");
        builder.Menus.Toggle(MenuPlace.Debug, "Spinner axes", "Draws each spinner's axis", true, (services, isOn) => ...);

        // A kind of node for the surface graph, and the HLSL it writes.
        builder.SurfaceNodes.Add(SpinUvNode.Definition);
    }
}
```

What its delegates are handed is the editor (`IEditor`): its window and dispatcher, the selection, the play state, the
project's folder, the scene on the engine's thread (`RunOnDeviceThread`, `SetComponent`), `MarkChanged` for the title's
unsaved mark, the panels and tools by type, and the generic inspector's field editors (`CreateFieldsView`) to build a
view from. A panel is an `EditorPanel` - its content, shown and closed as a document tab - and a tool an `EditorTool`, a
modal window with an Apply button where it has one.

Every public `IEngineEditorFeature` is the **editor's half** of a game feature: registered inside the engine right
after the feature goes in, in the editor only, for what needs the editor's side of the engine -

- **overlays**: steps of `UpdateStage.Overlays` drawing lines over the scene, through `DrawableLineBatch`;
- **viewport modes** (`builder.Viewport.Mode<T>()`): a way the mouse works in the viewport while the mode is picked;
- **snapshot keys** (`builder.Snapshots.Add`): a value of the game's carried with each snapshot the inspector reads, read
  back by its view as `snapshot.Extras.Get(key)`;
- **icons** over the entities of a kind without a mesh (`builder.Content.EmitterIcon<T>`), and the components,
  services and inputs an engine feature registers.

As the game's code is built again, the editor lets go of all of it: the game's open panels are closed and opened again
from the new build, its tools disposed, its menu items taken out and put back, the inspector made afresh, the surface
graphs written again with the new kinds of node. So editor code keeps nothing in static fields, unhooks what it hooks -
an event of the editor's, a timer - when its panel or tool is disposed, and builds its views in code or with XAML of
its own: a game's XAML types and dependency properties are let go of with its build. An editor part that fails - its
Register throwing, or a build made against a newer SDK than the editor's - is left out and the Game code panel says
why, the game's engine features running on; a panel, a tool, a view or a command that throws later is caught and said
there too.

## Collisions

An entity collides as its **collider components** - Box, Sphere, Capsule and Cylinder colliders in 3D; Rectangle, Circle
and Capsule 2D colliders in the world's XY plane, the side view a platformer is seen in - offset from its origin,
scaled and turned with it (a box or a rectangle stays axis-aligned while its entity is unturned). Each has **Is trigger**
- a trigger stops nothing and tells what comes into it - and two masks of 32 layers: **Layers**, the layers it is on, and
**Mask**, the layers it reacts to. 2D colliders meet only 2D ones, 3D only 3D. The inspector edits them; Add component
offers them; **Debug > Colliders** shows every collider's wires (solid green, triggers orange), the selection's always
showing; a collider with no mesh gets a marker icon.

A game's code asks `CollisionSystem` (a service: `services.Get<CollisionSystem>()`), on the engine's thread:

```csharp
builder.Update.Step("MyGame.player", UpdateStage.Animate, services =>
{
    var collisions = services.Get<CollisionSystem>();
    var velocity = Vector2.Zero;

    // player and goal: the entities, found as the game starts (builder.Play.OnChanged).

    return frame =>
    {
        var elapsed = frame.SceneClock.Elapsed;

        velocity.Y -= 20 * elapsed;                                              // gravity
        var moved = collisions.MoveAndSlide2D(player, velocity * elapsed);        // moves the player's entity

        if (moved.IsOnFloor) velocity.Y = 0;                                      // landed: it may jump

        foreach (var entered in collisions.TriggerEntered)                       // the last frame's, while playing
            if (entered.Other == player && entered.Trigger == goal) Finish();
    };
});
```

- `MoveAndSlide` / `MoveAndSlide2D` move a kinematic body - the entity, by its first solid collider of that dimension -
  stopping a skin short of the solid colliders on its Mask and sliding along them, and say whether it ended on a floor,
  against a wall or under a ceiling. They move the entity at once: its local position, and its colliders for the
  queries after.
- `Raycast` / `Raycast2D`, `Overlap` / `Overlap2D`, `Sweep` / `Sweep2D`: by a mask, with or without triggers, leaving an
  entity out.
- `TriggerEntered`, `TriggerExited`: what came into a trigger and went out of one at the last frame's update - only while
  the game plays, nothing to subscribe to or unhook as the code reloads. `IsInTrigger(trigger, other)` asks directly.

The colliders follow their entities once a frame, after the hierarchy is resolved; a body moved in a frame's update is
seen by every query after it at once. The math is the
[BorrowedTorch.Collisions](https://github.com/BorrowedTorchStudio/BorrowedTorch.Collisions) package's, which works on
its own too; frustum culling is BorrowedTorch.Culling's, on it.

## Surface graph

A material's look made as a graph rather than written in HLSL: **View > Surface graph**. Name a new surface and press
**+** - it starts as a color times the entity's own, with a roughness and a metalness - and pick it for a material in the
material dialog as any shader. Each change is compiled a moment later and shows at once on whatever uses the surface;
each node's picture shows what it gives, the output's the surface lit on a sphere (**Sphere previews** puts the others
on one too), with the values and textures of the first material using the surface - edited in the material dialog,
they follow as you edit.

- **Nodes** come from the node search: `Space` or right-click on the canvas, **Add node**, or a wire dragged from a
  port and let go on nothing - then only what can take it is offered. What the mesh gives (UV, position - on the object
  or in view - normal, view direction, the entity's color, the clock, opacity), the material's parameters, numbers and
  colors, Sample texture, Normal map and Parallax, math, vectors split and put together, UVs tiled, turned and moved,
  noise (gradient, fractal, 3D, cellular, random), sRGB to linear, luminance, Fresnel, height to normal, Custom code,
  the kinds a game's editor code adds (see Extending the editor) - and the **Surface**: base color, alpha, normal,
  metallic, roughness, occlusion, emission, base reflectivity, transmission. One of its inputs left alone keeps the
  surface's default.
- **Lighting** on the Surface is **Lit** or **Unlit**. An unlit surface takes no light, shadow or reflection: it shows
  its base color as it is, its emission on top - for effects, screens and flat, drawn looks - and keeps only the ports
  that still mean something (base color, alpha, normal, emission). It still hides and shadows what is behind it, and its
  color is light of its own to the rays: reflections show it as it is drawn, and it lights what is about it, as a glow
  does - unless the entity's Mesh panel says otherwise: **Casts shadow** off, the traced shadows pass it by; **Glow
  lights around** off, its glow (an unlit surface's color, any material's emission) is still seen, in reflections too,
  but lights nothing about it.
- **Parallax** gives relief from a height map: where the eye's ray meets it, as UV to read the other maps at (wire its
  UV into them), and how deep. Its height is in the map's red channel, or the alpha of a packed ORMH map (**Height in**);
  **White is low** for a map that has it the other way. With **Self-shadow** the relief shadows itself under each light
  - one Parallax node a surface, its **Height** from a parameter or a number. It needs a mesh with tangents.
- **Custom code** is HLSL of your own, for what no node does: its **Inputs** and **Outputs** listed as `name: type`
  (`float` to `float4`) apart by commas, its code reading the inputs by name and setting the outputs - each 0 until it
  does - with no `return`. It can call what the template has: `ToLinear`, the noise functions, `i.UV`, `i.Time`. An
  error in it is shown on the node with the line of yours it is on.
- **Wires** go from port to port; one dropped on a node goes into its first port that takes it; `Alt`+click on a wire
  or a port takes it off. A wire's color is its type: one to four floats, or a texture. A single float into a wider
  port goes into each part; a wider value gives the parts the port has room for, a narrower one has the rest filled
  with 0.
- **Values** are typed into an input nothing is wired into, or dragged sideways on.
- **Parameters**, on the right, are what a material using the surface sets, listed in its dialog in this order. A name
  has to be one HLSL and the engine let a material value have - the panel says why not until it is. A texture is a color
  map, read as sRGB, when its name ends in `ColorMap` (its switch renames it); any other is data - normals, masks,
  heights.
- **Subgraphs** are nodes of your own: name one under **Subgraphs** and press **+**, and it is in every surface's node
  search (and every other subgraph's) by that name. What it takes are its **Subgraph input** nodes - each a port of the
  node using it, top to bottom, of its name, type and default: one to four floats, a texture, or **UV** (the mesh's UV
  where nothing is wired in) - and what it gives are the ports listed on its **Subgraph outputs** node, as `name: type`.
  It has no parameters: a surface wires its own into it. A surface using one is written again as it is edited - its ports
  following, a wire into a port that is gone taken off - and its errors show on the node using it. A subgraph cannot use
  itself, nor one that uses it. Its pictures show it with its inputs' defaults. **Rename**, on the right, renames it and
  its uses in every graph with it. **-** deletes it; a node using it keeps its wires and gives nothing until a subgraph of
  that name is back. Its file goes - or, renamed, takes the new name - when the project is saved.
- `Delete`, `Ctrl+D` to duplicate, `Ctrl+C`/`Ctrl+V`, `Ctrl+Z`/`Ctrl+Y`. Errors show on the node they come from, and
  below the canvas.
- **Saved with the project** (`Ctrl+S`): `Assets\Shaders\<Name>\<Name>.btsurface` is the graph, `<Name>.fx` beside it
  the shader made from it, which the player compiles and draws as any shader. The shader editor shows that shader
  read-only, with **Open in graph**, or **Detach** to keep it as a shader of its own to edit as text (the graph file is
  deleted). A shader changed by hand since it was made is asked about when its graph is opened. Subgraphs are saved as
  `Assets\Subgraphs\<Name>.btsubgraph`; the shaders of the surfaces using one already hold its nodes, so the player
  needs no subgraph.

The surfaces are drawn as every lit material is (`Surface\LitTemplate.fxh`, an unlit one under `SURFACE_UNLIT`):
see-through, refraction, the traced shadows, occlusion and reflections all follow from what the Surface is given. A Normal map needs a mesh with
tangents; Height to normal does not.

## Controls

In the viewport:

| Input                     | Does                                                                    |
|---------------------------|-------------------------------------------------------------------------|
| Middle mouse button, drag | Look around - orbit, in the orbiting camera mode; pan, in the 2D view   |
| Mouse wheel               | Zoom: toward what is under the cursor; a chase's distance; in the 2D    |
|                           | view, its height, about the cursor                                      |
| `W` `A` `S` `D`           | Fly forward, left, back, right - in the 2D view, pan up, left, down,    |
|                           | right                                                                   |
| `C`, `Z`                  | Fly up, down                                                            |
| `Q`, `E`                  | Roll                                                                    |
| `Left Shift`, held        | Fly slowly - or pan slowly                                              |
| Left click                | Select what is under the cursor, past anything made unpickable          |
| `Ctrl` + left click       | Add what is under the cursor to the selection, or take it out           |
| Left mouse button, drag   | Frame entities: select every one whose middle is inside; `Ctrl` adds    |
| `Esc`                     | Clear the selection, leave pin or paint mode, or drop a frame           |
| `R`                       | Cycle the gizmo: move, rotate, scale                                    |
| `G`                       | Gizmo axes: world or the selection's own                                |
| `Ctrl`, held in a drag    | Snap the gizmo's drag the other way from the toolbar's Snap             |
| `1` `2` `3`               | Camera mode: free, chase, orbiting chase - out of the 2D view           |
| `4`                       | Play a camera path                                                      |
| `P`                       | Add a waypoint where the camera is: after the selected one, or anew     |
| `O`                       | Show or hide the camera paths                                           |
| `Ctrl` + `S`              | Save the project (anywhere in the editor)                               |
| `F5`                      | Save the project and play the scene in the player (anywhere)            |

The toolbar holds the rest: Play, Pause, Step and Stop - the scene's wind, clouds and cloth move only while it plays,
and Stop puts the scene back as it was played from - and the time multiplier beside them, how fast the scene's time runs:
from x0.125, slow motion, to x8, sped up, in doublings, x1 in the middle as the game runs. A Step is a frame at it,
the animations go by it too, and a click on its figure goes back to x1; it is kept for the session only, not saved.
Then the panes, the cameras and the 2D view, and how the scene is shown - wireframe, ambient light, bloom, tone mapping and exposure. The
Settings menu turns the traced light on and off - ambient occlusion, bounced light, reflections, refraction and frosted
glass, the lights' glow, the shadows and their softness, the grass's contact shadows - and anti-aliasing, NVIDIA DLSS in its
place where it is installed, VSync and the frame rate counter. The Hardware capabilities window lists the video cards
and the displays, and holds the render resolution: the viewport drawn as it would be at one of the display's
resolutions, and upscaled to its own size. The Debug menu has the ray view - what a ray from the eye finds, as bounced light will, shown in place of the scene - the
traces raw or traced every frame, the bounced light's cache and glow-aiming, and the grass's culling by cell and
behind walls, to compare each without it.

## License

MIT.
