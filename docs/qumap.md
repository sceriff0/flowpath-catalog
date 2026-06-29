# FlowPath – qUMAP

**UMAP dimensionality reduction and visualization** for QuPath. Embed all of a
cell's markers in 2D, color by phenotype, lasso subsets with interactive
polygons, and overlay marker expression — all on multiplexed imaging data
(CODEX, MIBI, mIF). UMAP is computed via the
[SMILE](https://haifengl.github.io/) library.

[:fontawesome-brands-github: Repository](https://github.com/sceriff0/qupath-extension-flowpath-qumap){ .md-button }
[:material-download: Install](installation.md){ .md-button }

<figure class="screenshot" markdown>
![UMAP embedding colored by phenotype](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>A UMAP embedding colored by phenotype, with a sortable legend of names and counts. <em>(placeholder)</em></figcaption>
</figure>

## At a glance

| | |
|---|---|
| **Launch** | Extensions → FlowPath - qUMAP (++ctrl+u++) |
| **Reads** | Cell measurements + PathClasses (mirrors GatingTree's compartment / statistic model for feature selection) |
| **Writes** | `umap_coordinates.csv` · population tags as derived PathClasses |
| **Compatibility** | QuPath ≥ 0.7.0 · MIT licensed |
| **Repo** | <https://github.com/sceriff0/qupath-extension-flowpath-qumap> |

## Features

- **UMAP embedding** — compute a 2D UMAP from all available markers using
  [SMILE](https://haifengl.github.io/).
- **Phenotype coloring** — cells colored by their PathClass (e.g. from
  [GatingTree](gatingtree.md)), with a sortable legend of names and counts.
- **Interactive polygon gating** — draw polygons in UMAP space to focus on a
  subset; out-of-polygon cells are temporarily marked **UNFOCUSED** (grey).
- **Population tagging** — name + color a selection and store it permanently as
  a QuPath derived PathClass (e.g. "CD4+: Cluster A"); multiple tags coexist
  with colored ring overlays.
- **Marker expression overlay** — side-by-side UMAP colored by z-score
  (blue-white-red) or raw intensity (viridis).
- **Configurable subsampling** — Auto / Off / Fixed modes with stratified
  sampling that preserves phenotype proportions.
- **OOM protection** — pre-computation memory estimation and graceful recovery
  from out-of-memory.
- **Zoom / Pan** — scroll-wheel zoom, middle-click drag pan, double-click reset.
- **Export** — UMAP coordinates and phenotypes to CSV.

<figure class="screenshot" markdown>
![Polygon selection with UNFOCUSED cells greyed out](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>Polygon selection in UMAP space; cells outside the polygon grey out as UNFOCUSED. <em>(placeholder)</em></figcaption>
</figure>

## Quick start

1. Open your pyramidal OME-TIFF in QuPath.
2. Get cell detections with marker intensities in (e.g. from
   [MIRAGE](mirage.md) or via [AnnoMask](annomask.md)).
3. `Extensions → FlowPath - qUMAP` (++ctrl+u++).
4. Adjust UMAP parameters (k, epochs) and subsampling mode for large datasets.
5. Click **Compute UMAP** and wait for the embedding.
6. Cells are colored by phenotype automatically if classified (e.g. from
   GatingTree).
7. Select a marker from the dropdown for a side-by-side expression overlay.
8. **Draw Polygon** → click to add vertices → double-click to close.
9. Enter a name + pick a color → **Apply Tag** to mark the selection
   permanently.
10. **Export CSV** to save UMAP coordinates with phenotype labels.

<figure class="screenshot" markdown>
![Marker expression overlay: z-score vs raw](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>Side-by-side overlay — z-score (blue-white-red) and raw intensity (viridis). <em>(placeholder)</em></figcaption>
</figure>

## Output format

**UMAP CSV** (`umap_coordinates.csv`) — one row per cell:

```csv
UMAP_X,UMAP_Y,Phenotype
-3.241519,1.872034,CD4+
2.109384,-0.543218,CD8+
0.012345,4.321098,Unclassified
```

## Performance

| Cell count | Strategy | Expected time |
|---|---|---|
| < 10K | Direct computation | 2–5 s |
| 10K–50K | Direct with progress indicator | 10–30 s |
| 50K–100K | Auto-subsampling recommended | 5–15 s |
| > 100K | Subsampling + kNN projection | 10–30 s |

qUMAP estimates memory usage before computing and warns if a dataset may cause
out-of-memory issues. Subsampling uses stratified random sampling to preserve
phenotype proportions, then projects the remaining cells via weighted
k-nearest-neighbor interpolation.

!!! tip "Gate first, then embed"
    Assign PathClasses in [GatingTree](gatingtree.md), then color the qUMAP by
    those classes — coherent populations appear as distinct islands, a quick
    visual check on your gating.
