# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vimage is a console-based image manipulator with a Vim-style modal UI, written in [Rogue](https://github.com/brombres/Rogue) (`.rogue` files). Rogue compiles to C, which is then compiled to a native executable.

## Commands

Building uses `rogo` (Rogue's build tool, installed via Morlock):

- `rogo` — build and run
- `rogo build` — incremental build (Rogue → `Build/Vimage-<OS>.c` → `Build/<OS>-<arch>/vimage`)
- `rogo rebuild` — force full rebuild
- `rogo debug` / `rogo release` — force-rebuild in that mode and run (default mode is release; can be overridden in an untracked `Local.settings` file)
- `rogo clean` — delete `Build/` and `.rogo/`
- `rogo update_version X.Y.Z` — bump `VERSION`/`DATE` in `Source/Vimage.rogue` and README.md
- `rogo help` — list all build commands

There is no automated test suite. `Test/` contains sample PNGs for manual testing: `rogo build && Build/*/vimage Test`.

`Libraries/Rogue` is a local, git-untracked copy of the Rogue toolchain/standard library that the build compiles against. The standard-library modules used (Graphics, UI, Console/ConsoleUI, Epilog, etc.) live there — look in `Libraries/Rogue/Source/Libraries/` when you need API signatures.

## Architecture

All source lives in `Source/`, in a single `library Vimage`; `Vimage.rogue` is the entry point and `$include`s every other file. Native image libs (libjpeg, libpng, etc.) are declared in `Build.rogue` `#$ LIBRARIES(...)` directives and auto-installed by rogo.

The app is a modal state machine dispatching command objects:

- **`State.rogue`** — modal states (`ImageListState`, `ZoomState`, `CropState`, `ExtrudeState`, `EnterCmdLineState`), all singletons extending `StandardState`/`State`. Each state fills `key_handlers`, a map of `keycode | SHIFT/CONTROL` bitflags → `Cmd`. Switch states via `state = SomeState` / `Cmd(SomeState)`. Numeric prefixes (`5n`) are accumulated into `Vimage.repeat_count` by `StandardState.on`.
- **`Cmd.rogue`** (~4200 lines) — every keystroke operation is a `Cmd` subclass. Key overrides: `execute` (the work), `is_edit` (true → undo state is saved before running and the command becomes `.`-repeatable), `allowed_in_macro`, `execute(repeat_count)` when repetition isn't a plain loop. All commands funnel through `Vimage.execute(cmd)`, which handles undo snapshots and macro recording.
- **`CmdLineHandler.rogue`** — `:` colon commands are `CmdLineHandler` subclasses (keyword + `execute(args:String)`), each of which must also be registered in `VimageUI.init`. The base class has shared parsers for colors (`parse_color`: V/RGB/ARGB/RRGGBB/AARRGGBB hex forms), anchors, and sizes. `EnterCmdLineState` does keyword completion/disambiguation against the registered handlers.
- **`VimageUI.rogue`** — `ConsoleUI` singleton assembling the widget tree: `TitleBar`, `ImageList` (left column `VList` of `ImageListItem`s), `ImageView` (renders the current bitmap as truecolor character blocks), `StatusBar`.
- **`Image.rogue` / `ImageList.rogue`** — `Image` wraps a lazily-loaded `Bitmap` plus file, zoom/camera, and `is_modified`. `ImageList` is the singleton list; most commands operate on `ImageList.selected.image` and the image "below/under" it (next in the list).
- **Undo/redo** (`Vimage.rogue`) — whole-`ImageList` snapshots serialized to `Variant` (`ImageList->Variant` / `unpack`), capped at 50.

## Conventions

- Commit message style: `[v1.38.1]` for releases, `[:cmdname]` or `[KeyName]` prefix describing the affected command for features/fixes.
- When adding or changing a key command, `:command`, filter, generator, or channel op, update the corresponding table in README.md (and ChangeLog.md for release notes).
- `Morlock/vimage.rogue` is the Morlock package definition (end-user install path); it rarely changes.
