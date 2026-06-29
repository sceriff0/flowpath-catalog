# FlowPath – AnnoMask

A QuPath 0.7+ extension that converts a **labeled TIFF segmentation mask into
QuPath detection objects** (and GeoJSON) without leaving the app. It's the
**alternative on-ramp** for getting cells into QuPath when all you have on disk
is a labeled mask — from [MIRAGE](mirage.md), Cellpose, StarDist, or a custom
pipeline.

[:fontawesome-brands-github: Repository](https://github.com/sceriff0/qupath-extension-annomask){ .md-button }
[:material-download: Install](installation.md){ .md-button }

<figure class="screenshot" markdown>
![AnnoMask dialog with input modes and intensity sampling](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>The AnnoMask dialog — choose an input mode and optionally sample per-channel intensities. <em>(placeholder)</em></figcaption>
</figure>

## At a glance

| | |
|---|---|
| **Launch** | Extensions → FlowPath - AnnoMask (++ctrl+shift+m++ / ++cmd+shift+m++) |
| **Reads** | A labeled `.tif` / `.tiff` mask (+ the OME-TIFF for intensity sampling) |
| **Writes** | QuPath detection objects / GeoJSON `FeatureCollection`, intensities keyed by bare channel name |
| **Compatibility** | QuPath ≥ 0.7.0 · MIT licensed |
| **Repo** | <https://github.com/sceriff0/qupath-extension-annomask> |

## What it does

Given a labeled mask — a single-band integer raster where `0` is background and
each positive value is a unique cell/region ID — AnnoMask traces the contour of
every label and produces one QuPath `PathDetectionObject` per label, using
QuPath's built-in `ContourTracing`.

It can also:

- **Sample mean intensity per channel per detection**, so downstream tools like
  [GatingTree](gatingtree.md) see populated measurements and can gate
  immediately. Intensity is computed with the **same bincount pass MIRAGE uses
  in `bin/quantify.py`** — the values are identical, and measurements are keyed
  by **bare channel name** (`CD45`, `DAPI`), which is exactly what GatingTree
  and qUMAP read.
- **Add the detections** straight into the current image's hierarchy.
- **Save** the result as QuPath-native GeoJSON.

!!! info "One detection per label"
    One detection is produced per unique integer label. Labels with multiple
    connected components are merged into a single detection — matching MIRAGE's
    cell count.

## Two input modes

=== "Channel in current image"

    Pick a channel whose pixel values are integer labels. Useful when
    segmentation output is merged into the OME-TIFF as an extra channel.

=== "Load from file"

    Pick a labeled `.tif` / `.tiff` on disk. It is assumed to align to the
    current image's pixel grid.

It also handles **Cellpose / StarDist** masks, not just MIRAGE's.

<figure class="screenshot" markdown>
![Before/after: label mask to QuPath detections](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>A label mask (left) and the QuPath detections AnnoMask produces from it (right). <em>(placeholder)</em></figcaption>
</figure>

## Quick start

1. Open the image (the OME-TIFF) in QuPath.
2. `Extensions → FlowPath - AnnoMask` (++ctrl+shift+m++).
3. Choose an input mode (channel-in-image or load-from-file) and select the
   mask.
4. Enable **intensity sampling** if you want gateable measurements right away.
5. Run — detections appear in the hierarchy; optionally save as GeoJSON.

## How it relates to the rest of FlowPath

- **[GatingTree](gatingtree.md)** — interactive phenotype gating; AnnoMask
  produces the detections it operates on.
- **[qUMAP](qumap.md)** — UMAP over detection measurements; AnnoMask + intensity
  sampling populates the measurements it embeds.
- **[MIRAGE](mirage.md)** — produces the labeled masks AnnoMask consumes;
  AnnoMask is the "import step" when you want MIRAGE's masks inside QuPath
  without running MIRAGE's `export_geojson.py`.

!!! success "Interchangeable with MIRAGE's GeoJSON"
    Because AnnoMask reuses the exact bincount from MIRAGE's quantification and
    the same [bare-channel keys](measurement-keys.md), a GeoJSON it produces is
    plug-compatible with one MIRAGE wrote. The two
    [on-ramps](architecture.md) really do land in the same place.
