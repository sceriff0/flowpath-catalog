# Compatibility

## Platform requirements

| Requirement | Version |
|---|---|
| **QuPath** | **≥ 0.7.0** (every FlowPath release pins `version_range.min` to `v0.7.0`) |
| **Java** | bundled with QuPath at runtime; **JDK 25** to build from source |
| **OS** | anywhere QuPath 0.7.0 runs (Windows, macOS, Linux) |

## Current extension versions

These are the latest versions published in
[`catalog.json`](https://github.com/sceriff0/flowpath-catalog/blob/main/catalog.json).
The catalog is the source of truth — when in doubt, check it (or QuPath's
extension manager, which reads it live).

| Extension | Latest | Launch | Minimum QuPath |
|---|---|---|---|
| [FlowPath - GatingTree](gatingtree.md) | **v1.9.1** | ++ctrl+g++ | v0.7.0 |
| [FlowPath - qUMAP](qumap.md) | **v0.10.1** | ++ctrl+u++ | v0.7.0 |
| [FlowPath - AnnoMask](annomask.md) | **v0.3.4** | ++ctrl+shift+m++ | v0.7.0 |

!!! info "Versions move; the catalog tracks them"
    The numbers above reflect the catalog at the time of writing. New releases
    are added to `catalog.json` as they ship — your QuPath extension manager
    will always offer the newest release compatible with your QuPath version.

## Upstream

| Project | Role | Notes |
|---|---|---|
| [MIRAGE](mirage.md) | Upstream pipeline | Produces the OME-TIFF, `cells.geojson`, and masks FlowPath consumes. Not required — [any compatible source](faq.md) works. Docs: <https://mirage-pipeline.readthedocs.io/> |
| [QuPath](https://qupath.github.io/) | Host platform | FlowPath extensions run inside QuPath 0.7.0+. |

## Libraries of note

- **[SMILE](https://haifengl.github.io/)** — the UMAP implementation used by
  [qUMAP](qumap.md).
- **QuPath `ContourTracing`** — used by [AnnoMask](annomask.md) to trace label
  contours into detections.

See [Citation](citation.md) for how to credit these.
