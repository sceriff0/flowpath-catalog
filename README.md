# flowpath-catalog — superseded

> [!IMPORTANT]
> **FlowPath moved to a single repo.** As of FlowPath **v2.0.0**, GatingTree and
> qUMAP are one extension, and `catalog.json` now lives alongside its source at
> **[qupath-extension-flowpath](https://github.com/sceriff0/qupath-extension-flowpath)**.
>
> This repo receives no further catalog updates.

## If you have the old catalog URL

In QuPath: **Extensions → Manage extensions → Manage extension catalogs**, remove
the old entry and add:

```
https://raw.githubusercontent.com/sceriff0/qupath-extension-flowpath/main/catalog.json
```

`raw.githubusercontent.com` does not redirect, so this is a manual step — there is
no way to forward the old URL.

While you are there, remove **FlowPath - GatingTree** and **FlowPath - qUMAP** and
install **FlowPath**. QuPath keys extensions by name and will not offer the fused
extension as an upgrade of either; leaving the old two installed gives you three
menu items and two independent cell indices that disagree with each other. Saved
gate trees (`.json`) load unchanged.

## Why the `catalog.json` here still works

It is deliberately left in place, still listing the v1-era extensions. Anyone who
has not migrated yet keeps a working install and a working update check for those
versions — they simply will not be offered FlowPath v2.0.0 or anything after it.

## Related extensions

Still maintained, in their own repos:

| Extension | What it does |
|---|---|
| [AnnoMask](https://github.com/sceriff0/qupath-extension-annomask) | Import labelled segmentation masks (MIRAGE, Cellpose, StarDist…) as QuPath detections. |
| [Decidware](https://github.com/sceriff0/qupath-extension-decidware) | Decision-support tooling for QuPath. |

## License

MIT.
