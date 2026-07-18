# Tk 8.7 with macOS fixes

This repository is a Tk 8.7 development snapshot of
[tcltk/tk](https://github.com/tcltk/tk) [`core-8-branch`](https://github.com/tcltk/tk/tree/core-8-branch) ([`67b601c`](https://github.com/tcltk/tk/tree/67b601c631c4dae6cd70083f033b1429d7e57427),
2026-05-22) plus a small set of macOS/aqua fixes, kept as
one commit per fix on the `macos-fixes` branch.  See
[MACOS-FIXES.md](MACOS-FIXES.md) for the list of fixes and build notes.

It is particularly handy when building GUI applications with Perl and
[Tcl::pTk](https://metacpan.org/pod/Tcl::pTk) on macOS: Tcl::pTk runs
nicely on top of Tk 8.7, and with 8.7 images can be displayed crisply
on Retina (HiDPI) displays.  Tk 8.6 cannot do this at all — its `photo`
images are drawn scaled up per screen point and come out blurry — while
8.7's new `nsimage` image type maps image pixels 1:1 to display pixels.

The stock 8.7 aqua port, however, has several rough edges
that hit such applications hard — the save panel opens with its name
field not accepting input (Tcl::pTk passes `-parent` automatically, so
every save dialog was affected), text in fonts like Hiragino Sans sits
below center, keyboard focus on ttk buttons is invisible, and disabled
button images are not dimmed.  This branch fixes those.

The fixes were made while porting
[KH Coder](https://github.com/ko-ichi-h/khcoder) (a Perl/Tcl::pTk
application for quantitative text analysis) to Tcl/Tk 8.7 on macOS.

The original Tk README follows.

---

# README:  Tk

This is the **Tk 8.7b1** source distribution.

You can get any source release of Tk from [our distribution
site](https://sourceforge.net/projects/tcl/files/Tcl/).

9.0 (production release, daily build)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/linux-build.yml/badge.svg?branch=main)](https://github.com/tcltk/tk/actions/workflows/linux-build.yml?query=branch%3Amain)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/win-build.yml/badge.svg?branch=main)](https://github.com/tcltk/tk/actions/workflows/win-build.yml?query=branch%3Amain)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/mac-build.yml/badge.svg?branch=main)](https://github.com/tcltk/tk/actions/workflows/mac-build.yml?query=branch%3Amain)
<br>
8.7 (in development, daily build)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/linux-build.yml/badge.svg?branch=core-8-branch)](https://github.com/tcltk/tk/actions/workflows/linux-build.yml?query=branch%3Acore-8-branch)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/win-build.yml/badge.svg?branch=core-8-branch)](https://github.com/tcltk/tk/actions/workflows/win-build.yml?query=branch%3Acore-8-branch)
[![Build Status](https://github.com/tcltk/tk/actions/workflows/mac-build.yml/badge.svg?branch=core-8-branch)](https://github.com/tcltk/tk/actions/workflows/mac-build.yml?query=branch%3Acore-8-branch)

## <a id="intro">1.</a> Introduction

This directory contains the sources and documentation for Tk, a
cross-platform GUI toolkit implemented with the Tcl scripting language.

For details on features, incompatibilities, and potential problems with
this release, see [the Tcl/Tk 8.7 Web page](https://www.tcl-lang.org/software/tcltk/8.7.html)
or refer to the "changes" file in this directory, which contains a
historical record of all changes to Tk.

Tk is maintained, enhanced, and distributed freely by the Tcl community.
Source code development and tracking of bug reports and feature requests
take place at [core.tcl-lang.org](https://core.tcl-lang.org/).
Tcl/Tk release and mailing list services are [hosted by
SourceForge](https://sourceforge.net/projects/tcl/)
with the Tcl Developer Xchange hosted at
[www.tcl-lang.org](https://www.tcl-lang.org).

Tk is a freely available open-source package.  You can do virtually
anything you like with it, such as modifying it, redistributing it,
and selling it either in whole or in part.  See the file
`license.terms` for complete information.

## <a id="tcl">2.</a> See Tcl README.md

Please see the README.md file that comes with the associated Tcl release
for more information.  There are pointers there to extensive
documentation.  In addition, there are additional README files
in the subdirectories of this distribution.
