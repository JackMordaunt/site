+++
date = '2025-07-17T11:21:04-03:00'
draft = false
title = 'Plato Desktop Client'
description = "Desktop game client in Go."
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
  <img src="/images/Plato/PlatoHome.webp" alt="Home Page" />
  <img src="/images/Plato/PlatoGame.webp" alt="Game Page" />
  <img src="/images/Plato/PlatoMulti.webp" alt="Multi Window" />
</div>


## Overview

A cross-platform desktop game client built for **Plato Team Inc.**, supporting Windows, macOS, and Linux. Though now discontinued, the project showcased a modern, multi-window architecture with a primary chat interface and independent game windows.

The application was built in **Go**, using the **Gio** immediate-mode GUI toolkit. It interfaced extensively with C and C++ libraries, including **ANGLE** (OpenGL ES), **V8** (JavaScript engine), **SQLite**, and **libwebp** for image decoding.

---

### My Role

As one of two primary developers, I contributed across the full stack, with a focus on architecture, performance, and cross-platform integration.

- **Reactive GUI Architecture**  
  Co-developed a highly concurrent, fully reactive GUI system using **Gio** and **SQLite**. Built a declarative state model with the database as the single source of truth, enabling seamless multi-window synchronization.

- **Tooling & Abstractions**  
  Created reusable libraries and tooling ([skel](https://git.sr.ht/~gioverse/skel)) for stream-based data handling and safe inter-window communication. This enabled clean separation of concerns and race-free concurrency.

- **Auto-Updater System**  
  Designed and implemented a binary patch-based self-updater for seamless client updates with minimal bandwidth usage.

- **Embedded Game Logic**  
  Integrated game runtime logic using **V8**, communicating over a custom binary protocol with a proprietary backend.

- **Deep OS Integration**  
  Delivered native features on both Windows and macOS:  
  - Implemented **Windows COM interfaces in pure Go** for AAC audio decoding and system toast notifications.  
  - Used **Objective-C and C** on macOS for native audio and system notification support.

---

This project demanded **low-level systems programming**, **high-concurrency architecture**, and custom GUI tooling—all within the Go ecosystem. It reflects my ability to deliver **production-ready, cross-platform software** with deep OS integration and thoughtful abstractions.

### Platforms

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <i class="fa-4x fa-brands fa-windows"></i>
  <i class="fa-4x fa-brands fa-apple"></i>
  <i class="fa-4x fa-brands fa-linux"></i>
</div>

### Languages

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/go.svg"/>
  <img style="max-height: 100px;" src="/images/brands/c.png"/>
</div>

### Tech

<div style="display: flex; flex-wrap: wrap; gap: 2rem;">
  <img style="max-height: 100px;" src="/images/brands/gio.svg" class="invert-me"/>
  <img style="max-height: 100px;" src="/images/brands/sqlite.svg"/>
  <img style="max-height: 100px;" src="/images/brands/opengl.svg"/>
  <img style="max-height: 100px;" src="/images/brands/v8.svg"/>
</div>

