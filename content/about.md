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

## Agents

I write software with coding agents, and I hold the result to the same
standard as anything written by hand. An agent is fast and confidently
wrong, so the useful engineering is in the mechanisms around it:
[review](https://mordaunt.dev/code/review/) is a hybrid gate, static
checks and narrow model readers, that gives an agentic loop fast,
converging feedback on every change before it is committed, and measures
the commit message too;
[brain-cli](https://mordaunt.dev/code/brain-cli/) gives agents a shared
memory of what was decided and why, so nothing is re-argued and nothing
is quietly redone; and every change to this domain's server boots the
real configuration in a virtual machine and walks it as a browser, the
go tool and git would, before it ships. The techniques move quickly and
I move with them. The gate does not.

## Direction

I am building toward a native stack that beats the web on resources
and competes with it on delivery: patch updates, a CPU-rendered GUI,
and a WebAssembly target that meets the browser on its own ground.
Offline first, with your data kept on your machine and never monetised,
because the surest way not to leak data is never to hold it. The
browser should be for web pages; applications belong to the machine.

Everything I publish lives at its origin on this domain, mirrored to
GitHub and sourcehut. The [work](../work/) page has the case studies
and the [code](https://mordaunt.dev/code/) index has the repositories.

[Get in touch](../contact/) if you have a program that needs to be
small, native and built to last.

---

Mordaunt is Norman French for biting, as in a remark. The family motto
is a line of Virgil, *Nec placida contenta quiete est*: not content with
quiet rest. The work is built in that spirit.
