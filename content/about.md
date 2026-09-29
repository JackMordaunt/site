+++
title = "About"
description = "Jack Mordaunt builds native desktop software and small, sharp tools in Go and Odin."
date = "2026-09-28"
portrait = "images/portrait.webp"
+++

I am Jack Mordaunt, an Australian software developer working remotely.
I build native desktop software and small, sharp command-line tools. I
work in Go and Odin by choice, Rust and C where they fit, and TypeScript
when the program is a web page.

The standard I build to is software that is lean, efficient, powerful
and sovereign. In practice that means a small dependency tree, native
platform APIs rather than a runtime that pretends they do not exist,
and binaries meant to build and run a decade on. The oldest thing here,
[icnsify](../work/icnsify/), is from 2018 and still maintained; the
rest has that to live up to. Where a well-made C library already
exists, I bind to it rather than rewrite it, because C is the lowest
common denominator every platform speaks. That is also what draws me to
Odin: a C alternative built for the joy of programming.

I prefer client-side work, the kind a person runs on their own machine,
and I am comfortable a long way down the stack: COM interfaces written
in pure Go for Windows audio and notifications, Objective-C bridges on
macOS, FFmpeg linked directly on Linux.

Day to day I also work full stack on the web: several front ends, a
Postgres backend and four third-party integrations between them. I
know that world well enough to choose native on purpose.

The world I am building toward is a native stack that beats the web on
resources and competes with it on delivery. That means updates shipped
as tiny patches, so releasing is frictionless; a GUI rendered on the
CPU, for the least resource use and the widest compatibility; and a
WebAssembly target that turns the same program into a progressive web
app, so it meets the browser on its own ground and on its own terms.
Some of it exists: the Plato client shipped its updates as binary
patches, and [jm](https://mordaunt.dev/code/jm/) updates itself. Patch
updates in jm, the CPU renderer and the browser target are ahead of
me, not behind. The browser should be for web pages. Applications
belong to the machine.

Everything I publish lives at its origin on this domain, mirrored to
GitHub and sourcehut. The [work](../work/) page has the case studies
and the [code](https://mordaunt.dev/code/) index has the repositories.

[Get in touch](../contact/) if you have a program that needs to be
small, native and built to last.
