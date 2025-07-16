+++ 
draft = false
date = 2025-07-16T12:08:03-03:00
title = "Hugo SVG Tricks"
description = "Styling SVG icons alongside font-awesome icons."
slug = ""
authors = ["Jack Mordaunt"]
tags = ["hugo", "svg", "css"]
categories = ["development"]
externalLink = ""
series = []
+++

## Configuration driven, dynamic SVG icons

Change the theme to see the effect.

{{< inline-svg name="self-tiny-white" >}}

TLDR; 

- inline the SVG content directly into the HTML (avoid `<img>` tags, they disable CSS)
- use `{{ readFile "assets/images/image.svg" | safeHTML }}` to load it from a file at build time
- that path can be driven from configuration
- ensure the SVG has width and height set to `100%` to allow CSS to scale them
- place the SVG under `assets` for Hugo to use in pipelines

SVG images are troublesome. They're ubiquitous and yet riddled with quirks. 

The goal: add a configuration driven (not static) SVG icon to fit alongside font-awesome icons, that can be styled with CSS. For example a favicon that changes colour based on the theme. 

Font Awesome doesn't include every possible icon, just common ones. It bakes those icons into a font and refers to theme via unicode code points. This is great for compactness and ease of use when the icon is available within the font, but doesn't translate well to third-party SVG icons. 

In addition, there's a stark difference between inlining an SVG via `<svg>` tags, or referencing the image via an `<img>` tag. The former can be styled by CSS, the latter cannot. This means you must acquire the real SVG image and prepare it to be inlined. Thankfully you can apply CSS to the SVG as a normal tag once inlined.

If all you need is a singular SVG, then you can copy it into your HTML and stop here. 

To drive these icons from configuration you can configure the path to the SVG file like so:

```toml
[[params.social]]
  name = "Sourcehut"
  svg = "assets/images/sourcehut.svg"
  url = "https://git.sr.ht/~jackmordaunt"
```

In your template you can then embed the SVG as follows: 

```html
<div>{{ readFile .svg | safeHTML }</div>
```

And you can style it as you like:

```html
<div style="fill: #000; transition: fill 0.5s ease-in;">{{ readFile .svg | safeHTML }}</div>
```

