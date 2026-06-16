# Overview

FlowPath is a suite of [QuPath](https://qupath.github.io/) 0.7.0 extensions for
**single-cell analysis of multiplexed tissue imaging**. The guiding idea: once
your cells are segmented and their markers quantified, the rest of the
analysis — gating, phenotyping, dimensionality reduction — should feel like
working in a flow-cytometry tool, but stay anchored to the tissue image.

## The mental model

Think of FlowPath as three tools around one shared object: **QuPath detections
that carry per-marker measurements**.

| Tool | Verb | One-line role |
|---|---|---|
| **[AnnoMask](annomask.md)** | *import* | Turn a labeled segmentation mask into QuPath detections (optionally with intensities). |
| **[GatingTree](gatingtree.md)** | *phenotype* | Gate those detections into named populations with a live hierarchical tree. |
| **[qUMAP](qumap.md)** | *explore* | Embed all markers in 2D, color by phenotype, lasso clusters. |

The **[FlowPath Catalog](catalog.md)** is not a tool — it's the *distribution
hub*. You add its URL once and QuPath offers to install and update all three.

## Where MIRAGE fits

[MIRAGE](mirage.md) is a Nextflow pipeline that takes raw whole-slide
microscopy and produces:

- a **pyramidal OME-TIFF** (the image you open in QuPath),
- a QuPath-native **`cells.geojson`** (every cell with raw intensities,
  optional per-compartment measurements, and z-scores), and
- labeled **segmentation masks** (`*_cell_mask.tif`, `*_nuclei_mask.tif`).

MIRAGE deliberately **stops at quantified cells** — it does not phenotype them.
That last mile is exactly what FlowPath is for.

!!! info "Two projects, one workflow"
    MIRAGE runs *before* QuPath, as a pipeline. FlowPath runs *inside* QuPath,
    interactively. They meet at the OME-TIFF + measurements. MIRAGE has its own
    [documentation site](https://mirage.readthedocs.io/); this site documents
    the QuPath side.

## Two on-ramps

There are two ways to get MIRAGE's cells into QuPath with measurements attached:

=== "On-ramp A — import the GeoJSON"

    The fastest path. MIRAGE already quantified the cells, so open the OME-TIFF
    in QuPath and import `cells.geojson` directly. Detections arrive with all
    their marker measurements.

=== "On-ramp B — import the mask via AnnoMask"

    Skip MIRAGE's GeoJSON export and bring the **label mask**
    (`*_cell_mask.tif`) into QuPath via [AnnoMask](annomask.md), which
    re-derives identical intensities in-app using the same bincount pass MIRAGE
    uses.

Both routes end in the same place: QuPath detections with marker measurements,
ready for [GatingTree](gatingtree.md) and [qUMAP](qumap.md). The full data flow
is laid out in [Architecture](architecture.md).

!!! tip "You don't need MIRAGE"
    FlowPath works with **any** source of detections + measurements — Cellpose
    or StarDist masks via AnnoMask, or any GeoJSON with per-marker
    measurements. MIRAGE is the reference upstream, not a requirement.

## What you'll produce

By the end of a FlowPath session you'll typically have:

- **PathClasses** on your cells (named phenotypes from GatingTree),
- a saved **gate hierarchy** (`flowpath.json`) you can reload or share,
- a **phenotype table** (`gate_pheno.csv`), and
- **UMAP coordinates** (`umap_coordinates.csv`) with phenotype labels.

Ready? [Install the suite](installation.md), then follow the
[walkthrough](walkthrough.md).
