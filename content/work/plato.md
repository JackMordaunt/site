+++
title = "Plato desktop client"
description = "A multi-window desktop game client in Go, with native audio and notifications on Windows and macOS."
date = 2025-07-17
weight = 1
client = "Plato Team Inc."
role = "One of two primary developers"
year = "2023 to 2025"
status = "Discontinued in 2025"
platforms = ["Windows", "macOS", "Linux"]
stack = ["Go", "Gio", "SQLite", "V8", "ANGLE", "libwebp", "C", "Objective-C"]
+++

<div class="shots">
  <img src="/images/Plato/PlatoHome.webp" alt="The home window" width="560">
  <img src="/images/Plato/PlatoGame.webp" alt="A game window" width="560">
  <img src="/images/Plato/PlatoMulti.webp" alt="Several windows open at once" width="560">
</div>

A cross-platform desktop client for Plato's games: a primary chat window
and any number of independent game windows, on Windows, macOS and Linux.
Built in Go with the Gio immediate-mode toolkit, and bound directly to
the C and C++ libraries it needed: ANGLE for OpenGL ES, V8 for the game
runtime, SQLite for state, libwebp for images. The product is
discontinued; the architecture is the part worth describing.

## What I built

**A reactive, multi-window architecture.** Every window is a view over
one SQLite database, which is the single source of truth. State changes
are written once and every window that shows them updates, so
multi-window synchronisation is a consequence of the design rather than
a feature. The stream and inter-window tooling became a reusable
library, [skel](https://git.sr.ht/~gioverse/skel), which keeps the
concurrency race-free by construction.

**A self-updater on binary patches.** Clients update themselves with a
patch against the running binary rather than a fresh download, which
made updates cheap enough to ship often.

**The game runtime, embedded.** Game logic runs in V8 inside the client
and speaks a binary protocol to the backend, so a game is data the
client receives, not a build it ships.

**Native where it matters.** The Windows COM interfaces for AAC audio
decoding and toast notifications are written in pure Go, with no cgo.
The macOS equivalents are Objective-C and C. The same techniques are
open source on this domain, in
[nativeaudio](https://mordaunt.dev/code/nativeaudio/) and
[go-toast](https://mordaunt.dev/code/go-toast/).

## What it took

Low-level systems work, a high-concurrency architecture and custom GUI
tooling, all inside the Go ecosystem and all shipped to people's
desktops. The product was discontinued in 2025. It remains the clearest
example of the kind of program I build.
