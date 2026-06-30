# FlowPath – GatingTree

Interactive, **tree-based cell phenotyping** for QuPath. Build a hierarchy of
marker gates — for example `CD45+ → CD3+ → CD8+ = "T cytotoxic"` — and every
cell flows down the tree into a named phenotype, recoloring live as you work.

GatingTree is the primary consumer of [MIRAGE](mirage.md)'s `cells.geojson`,
and is *"designed to work with the mirage pipeline … from raw images to cell
phenotypes."*

[:fontawesome-brands-github: Repository](https://github.com/sceriff0/qupath-extension-flowpath-gatingtree){ .md-button }
[:material-download: Install](installation.md){ .md-button }

<figure class="screenshot" markdown>
![GatingTree hierarchy with live-recolored cells](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>A multi-level gate tree with cells recolored by phenotype in the viewer. <em>(placeholder)</em></figcaption>
</figure>

## At a glance

| | |
|---|---|
| **Launch** | Extensions → FlowPath - GatingTree (++ctrl+g++) |
| **Reads** | Detections with marker measurements — per-compartment keys (`marker: Compartment: Statistic`) and z-scores |
| **Writes** | `flowpath.json` (gate hierarchy) · `gate_pheno.csv` (per-cell phenotype + per-marker ± status) |
| **Compatibility** | QuPath ≥ 0.7.0 · MIT licensed |
| **Repo** | <https://github.com/sceriff0/qupath-extension-flowpath-gatingtree> |

## Features

- **Hierarchical gating** — multi-level gate tree (e.g. `CD45+ → CD3+ → CD8+ =
  "T cytotoxic"`).
- **Multiple gate types** — threshold (1D), quadrant (2D dual-threshold),
  polygon, rectangle, and ellipse.
- **Live histogram & scatter** — per-marker histogram with a draggable
  threshold; 2D scatter plots with interactive gate drawing and a crosshair for
  quadrant gates.
- **Real-time preview** — cells update color and phenotype within ~100 ms.
- **Raw / Z-score toggle** — switch value modes per gate (the z-score uses
  MIRAGE's own z-scores).
- **Quality filters** — pre-gating QC with min + max for area, eccentricity,
  solidity, total intensity, and perimeter.
- **Outlier exclusion** — per-gate percentile clipping, with the scatter plot
  axis zooming to the clipped range.
- **Undo / Redo** — snapshot-based stack (++ctrl+z++ / ++ctrl+shift+z++).
- **Drag-and-drop reordering** — rearrange gates between branches.
- **Per-gate compartment & statistic** — choose Nucleus / Cytoplasm / Cell and
  Mean / Median / Sum per gate.
- **Save / Load / Export** — gate trees as JSON (backward-compatible),
  phenotypes as CSV.

## Quick start

1. Open your pyramidal OME-TIFF in QuPath.
2. Get cell detections in (import `cells.geojson`, or use [AnnoMask](annomask.md)
   — see [Architecture](architecture.md)).
3. `Extensions → FlowPath - GatingTree` (++ctrl+g++).
4. Set **quality filters** to remove segmentation artifacts.
5. Add a **root gate** → pick a gate type.
6. Threshold gate: select a channel → drag the threshold on the histogram.
7. 2D gate: select X/Y channels → draw a region on the scatter plot.
8. Add **child gates** to branches for sub-gating.
9. **Name** leaf nodes ("T cytotoxic", "Stroma").
10. **Export CSV**.

<figure class="screenshot" markdown>
![1D histogram with draggable threshold](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>A 1D marker histogram with a draggable threshold. <em>(placeholder)</em></figcaption>
</figure>

<figure class="screenshot" markdown>
![2D scatter / quadrant gate](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>A 2D scatter plot with a quadrant gate and crosshair. <em>(placeholder)</em></figcaption>
</figure>

## Output formats

**GatingTree JSON** (`flowpath.json`) — saves the full gate hierarchy,
thresholds, colors, and quality-filter settings. Load it to reproduce gating.

**Phenotype CSV** (`gate_pheno.csv`) — one row per cell, phenotype name plus
per-marker ± status:

```csv
cell_id,phenotype,CD45,CD3,CD8,PANCK
0,T cytotoxic,+,+,+,-
1,Tumor,-,-,-,+
```

Cells excluded by QC or outlier clipping are omitted when "Exclude from CSV" is
enabled.

!!! success "Native to MIRAGE's measurements"
    GatingTree understands MIRAGE's [per-compartment keys](measurement-keys.md)
    natively, and falls back to whole-cell / Mean for legacy data — so you pick
    compartment and statistic per gate without any reformatting.

!!! tip "Pair it with qUMAP"
    After gating, open [qUMAP](qumap.md) and color the embedding by the
    PathClasses GatingTree assigned to check whether your gates form coherent
    islands.
