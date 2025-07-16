+++ 
draft = false
date = 2025-07-16T14:57:56-03:00
title = "Hugo and nip-05"
description = "Setup nostr nip-05 with a Hugo site."
slug = ""
authors = ["Jack Mordaunt"]
tags = ["hugo", "nostr"]
categories = ["development"]
externalLink = ""
series = []
+++

[nip-05](https://nostr.how/en/guides/get-verified) is a convenience for identifying users on nostr such as `mordaunt@jackmordaunt.com`, which is 
shorter and more memorable than a {{< tooltip tip="npub14l0387z0ejfmnn3j38nnalg5p57952d84aqakqguhzx7p7pns2vqg3x78w" >}}raw npub{{< /tooltip >}}.

It works by making a `GET` request at `jackmordaunt.com/.well-known/nostr.json` and extracting the
public keys from the JSON response.

This quick guide will show you how to host your own with a Hugo site.

### Add your configuration.

Inside your `hugo.toml`, add:

```toml
[[params.nostr]]
  name = "name"
  npub = "npub..."
```

If you have multiple names you want to host you can add them.

```toml
[[params.nostr]]
  name = "name1"
  npub = "npub..."
[[params.nostr]]
  name = "name2"
  npub = "npub..."
[[params.nostr]]
  name = "name3"
  npub = "npub..."
```

I've picked `nostr` as the subkey, but you can structure this config however you like.

### Define the output type.

```toml
[outputFormats.nostrjson]
  mediaType = "application/json"
  baseName = "nostr"
  path = ".well-known"
  isPlainText = true
  permalinkable = true
  notAlternative = true

[outputs]
  home = ["HTML", "nostrjson"]
```

This output type tells Hugo that we want to render a JSON file.
`nostrjson` is just the name of this type, you can pick whatever name you like.

Read more about output formats [here](https://gohugo.io/configuration/output-formats).

### Add the JSON template

```html
{{ $nostr := .Site.Params.nostr }}
{
    "names": {
      {{- range $k, $v := $nostr }}
        "{{ $v.name }}": "{{ printf "%x" $v.npub }}"{{ if not (eq $k (sub ($nostr | len) 1)) }},{{ end }}
      {{- end }}
    }
}
```

This template iterates all nostr configurations under `.Site.Params.nostr` and renders a key-value entry
in the JSON object. The npub is hex encoded because that is the expected format.

The complex conditional at the end is simply for placing the commas correctly.

### Build it

`hugo build` and verify that the root of the output under `public` contains `.well-known/nostr.json` with
the correct content!

When you publish the site you should be able to get the JSON from doing a `GET` for `.well-known/nostr.json`.

Lastly, you can specify relays that you are likely to be found on. You'll have to adjust the template.

```html

{
  "names": {
    "YOUR_NOSTR_NAME": "YOUR_NOSTR_PUBLIC_KEY_IN_HEX_FORMAT"
  },
  "relays": {
    "YOUR_NOSTR_PUBLIC_KEY_IN_HEX_FORMAT": [
      "wss://relay.one",
      "wss://relay.two",
      ...
    ]
  }
}
```
