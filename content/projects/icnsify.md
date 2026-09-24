+++
date = '2018-02-21T12:14:44-03:00'
draft = false
title = 'Icnsify'
description = "Build and preview .icns icons cross platform."
tags = ["Go", "Gio", "Portfolio"]
categories = ["development"]
+++

<style>
.carousel {
  display: flex;
  overflow: scroll;
}
</style>

<div class="carousel">
  <img src="/images/icnsify/icns-macos-preview.webp" alt="icnsify-preview macOS" />
  <img src="/images/icnsify/icns-windows-preview.webp" alt="icnsify-preview Windows" />
</div>

## icnsify: A Lean, Cross-Platform Icon Tool for macOS `.icns` Format  
[GitHub →](https://github.com/JackMordaunt/icns)

### Overview  
`icnsify` is a fast, lightweight utility for creating and previewing `.icns`
files—the proprietary Apple Icon Image format—without relying on macOS.

I built the first version in a Swedish café in 2018 as my first serious
open-source contribution. Since then, it has grown into a widely used
tool among developers—especially those building cross-platform apps with
[Gio](https://gioui.org)—to automate icon generation as part of their CI
pipelines or local development.

The native Apple tool, `iconutil`, is macOS-only, hard to script, and requires a
rigid folder structure with pre-sized PNGs. `icnsify` simplifies this:
- **Cross-platform** (runs on Linux, macOS, Windows)  
- **CLI-first** with support for piping and scripting  
- **Intelligently resizes a single image** to required sizes  
- **Auto-converts common formats to PNG**  
- **Preview GUI** for inspecting `.icns` on any OS  

Install with a single Go command: `go install github.com/jackmordaunt/icns/cmd/icnsify@latest`

### My Role  
I designed and implemented `icnsify` from scratch, using the `.icns` format
specification and reverse-engineering documentation like the Wikipedia entry.
Key design goals:
- **Keep it lean:** Minimal dependencies, instant startup, small binary  
- **Make it ergonomic:** Smart defaults, clear CLI, and usable in Unix pipelines  
- **Simplify a complex task:** Resize and convert images automatically  
- **Support the ecosystem:** Designed for developers publishing to macOS from any OS  

### Results  
`icnsify` is now used in multiple open-source projects targeting macOS,
particularly in the Go/Gio ecosystem. It has become a go-to tool for developers
who value simplicity, cross-platform compatibility, and automation in their
toolchain.

### Used By

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <a href="https://github.com/fyne-io">
    <img style="max-height: 100px; border-radius: 8px;" src="/images/brands/fyne-io.webp" alt="Fyne"/>
  </a>
  <a href="https://github.com/wailsapp/wails">
    <img style="max-height: 100px;" src="/images/brands/wails.webp" alt="Wails"/>
  </a>
</div>

### Platforms

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <i class="fa-4x fa-brands fa-windows"></i>
  <i class="fa-4x fa-brands fa-apple"></i>
  <i class="fa-4x fa-brands fa-linux"></i>
</div>

### Languages

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/go.svg"/>
</div>

### Tech

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/gio.svg" class="invert-me"/>
</div>

