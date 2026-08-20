# Tk 8.7 macOS fixes

This repository is a Tk 8.7 development snapshot of
[tcltk/tk](https://github.com/tcltk/tk) `core-8-branch`
(base commit `67b601c`, 2026-05-22) plus a small set of macOS/aqua fixes, made
while porting [KH Coder](https://github.com/ko-ichi-h/khcoder) to
Tcl/Tk 8.7 on macOS.  Each fix is a single commit on the `macos-fixes`
branch, so `git log core-8-branch..macos-fixes` shows exactly what was
changed and why.

Tcl is used unmodified: an upstream
[tcltk/tcl](https://github.com/tcltk/tcl) `core-8-branch` development
snapshot (commit `b37de7b`, 2026-05-22).

## The fixes

| Commit | Fix | Files |
|---|---|---|
| `0f4f7aa` | Save/open panel opened as a sheet (via `-parent`) with its name field inactive: typing did nothing and Return had to be pressed twice. Run the panel free-floating and activate the app so the name field is focused immediately. Also stop duplicating `-title` into the message. | `macosx/tkMacOSXDialog.c` |
| `9b53dd2` | Fonts with large leading (Hiragino Sans reports 0.5em) drew text below center in labels, buttons, entries and menus. Distribute the leading evenly between ascent and descent; for menu items fall back to the plain title, since AppKit's centering of custom attributed titles has the same defect (`TK_MENU_KEEP_CUSTOM_FONT=1` restores the old behavior). | `macosx/tkMacOSXFont.c`, `macosx/tkMacOSXMenu.c` |
| `8bf37da` | ttk buttons, checkbuttons and radiobuttons showed no focus indicator at all under aqua. Draw the standard focus ring when `TTK_STATE_FOCUS` is set, and align the entry focus ring with the field border. | `macosx/ttkMacOSXTheme.c` |
| `5e4681d` | HITheme pads push buttons by ~20px per side, making text-sized buttons far too wide. Use a 6px horizontal pad for push buttons, and give `TButton` a default minimum width of 8 characters so very short labels don't yield tiny buttons (`-width`, including `-width 0`, still overrides). | `macosx/ttkMacOSXTheme.c`, `library/ttk/aquaTheme.tcl` |
| `c1d122b` | Disabled ttk images were drawn at full strength on aqua (the stipple-based dimming used on X11/Windows is not implemented there). Add `TkMacOSXFillRectAlpha` and wash disabled images out with the background color at 50% alpha. | `generic/ttk/ttkLabel.c`, `macosx/tkMacOSXDraw.c` |
| `cd15033` | Push buttons were drawn at a fixed standard height under aqua, so a large `-font` left the label spilling out above and below the button. Give the button element a `-font` option, measure the label's line height, and grow the button graphic to enclose it (plus a 1px margin) only when that exceeds the standard height, so small and normal fonts (e.g. size 9) keep the standard height exactly. The graphic is centered symmetrically so the focus ring, drawn 2px outside it, is not clipped, and a standard-height button given extra vertical room (e.g. `-sticky ns` with row `-weight`) stays centered at the standard height, matching native Aqua. | `macosx/ttkMacOSXTheme.c` |

## Building (macOS)

The standard macOS build via `macosx/GNUmakefile` works unchanged, e.g.:

```sh
# Tcl and Tk checked out side by side in src/tcl and src/tk
cd src/tk/macosx
make -f GNUmakefile deploy INSTALL_ROOT="$PREFIX"
```

Note that `library/ttk/aquaTheme.tcl` is embedded into the Tk library via
zipfs at build time, so changes to it also require a rebuild of Tk, not
just copying the script.
