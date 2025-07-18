+++
date = 'Thu Mar 17 17:30:23 2022'
draft = false
title = 'Sprig Arbor Chat Client'
description = "Arbor Chat client in Go."
authors = ["Jack Mordaunt"]
tags = ["Go", "Gio", "Native", "Portfolio"]
categories = ["development"]
+++

<style>
.carousel {
  display: flex;
  overflow: scroll;
}
</style>

<div class="carousel">
  <img src="/images/sprig/sprig.webp" alt="Sprig" />
</div>

## Overview

Built by [~whereswaldon](https://git.sr.ht/~whereswaldon), [Sprig](https://git.sr.ht/~whereswaldon/sprig) is a client for [Arbor](https://man.sr.ht/~whereswaldon/arborchat), an open source federated chat platform.  
Sprig is a cross-platform client supporting Windows, macOS, Linux, Android, and iOS.  
It's written in **Go**, using the [**Gio**](https://git.sr.ht/~eliasnaur/gio) immediate-mode UI toolkit.

---

### My Role

When I joined, Sprig stored each chat message as an individual file on disk. This file-per-message model worked reasonably well on Linux, but resulted in **severely degraded performance** on Windows, where reading thousands of tiny files led to **notably slow startup times**.

To address this, I migrated the persistence layer from the filesystem to [**BoltDB**](https://github.com/boltdb/bolt), an embedded key-value store written in Go. This upgrade significantly improved startup performance across all platforms, with the largest gains on Windows.

In addition to this core performance enhancement, I contributed minor UI improvements and refinements.

---

### Result 

The app now launches significantly faster, offers a smoother user experience, and maintains cross-platform compatibility—all without sacrificing the simplicity of a local-first storage model.

### Platforms

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <i class="fa-4x fa-brands fa-windows"></i>
  <i class="fa-4x fa-brands fa-apple"></i>
  <i class="fa-4x fa-brands fa-linux"></i>
  <i class="fa-4x fa-brands fa-android"></i>
</div>

### Languages

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/go.svg"/>
</div>

### Tech

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/gio.svg" class="invert-me"/>
  <img style="max-height: 100px;" src="/images/brands/boltdb.webp"/>
</div>

