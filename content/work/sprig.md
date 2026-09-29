+++
title = "Sprig"
description = "Made a cross-platform chat client start fast on Windows by replacing file-per-message storage with an embedded database."
date = 2022-03-17
weight = 2
client = "Arbor, open source"
role = "Contributor"
year = "2022"
platforms = ["Windows", "macOS", "Linux", "Android", "iOS"]
stack = ["Go", "Gio", "BoltDB"]
source = "https://git.sr.ht/~whereswaldon/sprig"
+++

<div class="shots">
  <img src="/images/sprig/sprig.webp" alt="Sprig" width="560">
</div>

[Sprig](https://git.sr.ht/~whereswaldon/sprig) is a client for
[Arbor](https://man.sr.ht/~whereswaldon/arborchat), a federated chat
platform, written in Go with Gio and shipped to five platforms from one
codebase.

## The problem

Sprig stored every chat message as its own file. Linux tolerated that.
Windows did not: reading thousands of small files at startup made the
client take seconds to open, and the cost grew with every conversation.

## What I did

I moved persistence from the filesystem to
[BoltDB](https://github.com/boltdb/bolt), an embedded key-value store in
pure Go, keeping the local-first model and the simplicity that came with
it. Startup became fast on every platform, with the largest gain on
Windows, where the problem had been worst. Alongside it, smaller
refinements to the interface.

## Why it is here

It is a small change with a large effect, made by measuring where the
time went rather than guessing, and it left the program simpler than it
found it.
