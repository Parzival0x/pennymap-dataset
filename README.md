# pennymap-dataset

Published data for the **Penny Machine Map** Android app. This repository holds exactly one
meaningful file:

- **`dataset.json`** — the current Dataset_File the app fetches on open.

## What this is

A machine-generated artifact. It is produced off-device by the Pressed Penny Collector companion
service (scrape → normalize → coordinate enrichment → geocode → assemble → version → publish) and
pushed here as a single-file commit after a successful run. **Do not edit it by hand** — the next run
overwrites it, and the app rejects a malformed document rather than importing it.

## What the app does with it

On open, the app fetches `dataset.json` over unauthenticated HTTPS and imports it **only if its
`datasetVersion` is strictly newer** than the installed one. A fetch failure, a malformed document or
an older version all leave the last-good dataset in place. If this file stops being updated, the app
surfaces the dataset's age rather than silently showing stale data.

Raw URL the app reads:

```
https://raw.githubusercontent.com/Parzival0x/pennymap-dataset/main/dataset.json
```

## What is deliberately *not* here

- **No personal data.** Collection status, personal notes, dates collected and captured photos are
  device-local and never leave the phone. The publisher refuses to push a document containing any of
  those fields.
- **No image bytes.** Machine photos are referenced by remote URL.
- **No scrape state.** The `.photos.json` and `.geocode.json` cache sidecars stay on the server.

## Document shape

`formatVersion`, `datasetVersion` (strictly increasing), `generatedAt` (ISO-8601), and a `machines`
array. The current `formatVersion` is 4; the app also still accepts 2 and 3, so an older document
imports unchanged.

## Provenance

Machine data is derived from public listings on [pennycollector.com](https://www.pennycollector.com/),
with coordinates cross-referenced against [PennyMe](https://github.com/webalorn/PennyMe) and geocoded
via [Nominatim](https://nominatim.org/) where no coordinate was available. UK operational status is
refreshed from [scottishpennies.com](https://scottishpennies.com/) when its confirmation is more
recent. Please respect those sources' terms if you reuse this file.
