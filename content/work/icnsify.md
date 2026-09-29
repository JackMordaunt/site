+++
title = "icnsify"
description = "A cross-platform tool for building and previewing macOS .icns icons, used in the Go desktop ecosystem's build pipelines."
date = 2018-02-21
weight = 3
client = "Open source"
role = "Author"
year = "2018 to now"
status = "Maintained"
platforms = ["Windows", "macOS", "Linux"]
stack = ["Go", "Gio"]
source = "https://mordaunt.dev/code/icns/"
+++

<div class="shots">
  <img src="/images/icnsify/icns-macos-preview.webp" alt="The preview on macOS" width="560">
  <img src="/images/icnsify/icns-windows-preview.webp" alt="The preview on Windows" width="560">
</div>

Apple's `.icns` format has one official tool, `iconutil`, which runs
only on macOS, resists scripting, and wants a rigid folder of pre-sized
PNGs. icnsify replaces it with one command that runs anywhere:

```
brew install jackmordaunt/tap/icnsify
icnsify -i logo.png -o Icon.icns
```

It takes a single image, produces every size the format wants, converts
common formats on the way in, and pipes like a Unix tool. A small GUI
previews an `.icns` on any operating system.

## Where it is used

[Fyne](https://github.com/fyne-io) and
[Wails](https://github.com/wailsapp/wails) generate their icons with it
when building macOS applications from Linux and Windows. I wrote the first version in a café in Sweden in 2018 as my first
serious open-source release, from the format specification and public
documentation, and it has been maintained since.

## What it stands for

Minimal dependencies, instant startup, a small binary, and a task that
was awkward made ordinary. The library behind it is
[icns](https://mordaunt.dev/code/icns/), with a Rust port,
[icns-rs](https://mordaunt.dev/code/icns-rs/).
