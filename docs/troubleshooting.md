# Troubleshooting

Common issues and how to resolve them. If something here doesn't cover your
problem, open an issue on the relevant extension repo (see [Changelog](changelog.md)
for links).

## Catalog & installation { #catalog }

??? failure "The catalog adds, but no extensions appear"
    Almost always a **QuPath version below 0.7.0**. Every FlowPath release pins
    a minimum of `v0.7.0`, so older QuPath won't show them. Check **Help →
    Show installation directory / About** for your version and upgrade if
    needed. See [Compatibility](compatibility.md).

??? failure "Adding the catalog URL fails or shows nothing"
    Check the URL for typos — it must be exactly:
    ```
    https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
    ```
    It must be the **raw** GitHub URL (note `raw.githubusercontent.com`), not
    the repository web page. A network/proxy block on `githubusercontent.com`
    will also cause this.

??? failure "Install starts but the extension never loads"
    The release JAR may have failed to download (e.g. a moved asset). Try the
    [manual JAR install](installation.md#alternative-drop-in-a-jar) from the
    extension's Releases page, and restart QuPath fully.

??? failure "Extension menu entry is missing after install"
    Restart QuPath completely — extensions register at startup. If it's still
    missing, confirm the JAR landed in QuPath's extensions directory
    (Extensions → Manage extensions shows the path).

## GatingTree { #gatingtree }

??? failure "Gates show no cells / histograms are empty"
    Your detections probably have **no measurements**. Import a `cells.geojson`
    that carries intensities, or use [AnnoMask](annomask.md) with **intensity
    sampling enabled**. See [Measurement Keys](measurement-keys.md).

??? failure "I can't pick a compartment (Nucleus / Cytoplasm / Cell)"
    The data has no [per-compartment keys](measurement-keys.md) — GatingTree
    falls back to whole-cell / Mean. Re-export from MIRAGE with per-compartment
    quantification if you need them.

??? failure "Too many tiny/odd objects are being gated"
    Set the **quality filters** (area, eccentricity, solidity, perimeter, total
    intensity) before gating to drop segmentation artifacts.

## qUMAP { #qumap }

??? failure "Out-of-memory while computing UMAP"
    qUMAP estimates memory first and warns you. Switch subsampling to **Auto**
    or **Fixed**, or increase QuPath's max memory (Edit → Preferences, or the
    QuPath memory setting) and restart. Large slides (>100K cells) should use
    subsampling + kNN projection.

??? failure "UMAP isn't colored by phenotype"
    Cells need **PathClasses** to color by. Gate them in
    [GatingTree](gatingtree.md) first, then recompute or recolor the embedding.

??? failure "Computation is very slow"
    Check the [performance table](qumap.md#performance) and enable subsampling
    for large datasets; reduce epochs for a faster (coarser) embedding.

## AnnoMask { #annomask }

??? failure "Detections are misaligned with the image"
    A loaded mask is assumed to **align to the current image's pixel grid**.
    Make sure the mask resolution and orientation match the open OME-TIFF.

??? failure "Cell count differs from what I expected"
    AnnoMask produces **one detection per unique integer label**, merging
    multiple connected components of the same label — matching MIRAGE's count.
    Re-label the mask if you want components split.

??? failure "Measurements are empty after import"
    Enable **intensity sampling** in the AnnoMask dialog and make sure the
    OME-TIFF (with the channels) is the open image.
