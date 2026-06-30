# Troubleshooting & FAQ

Common questions first, then fixes for specific issues. If nothing here covers
your problem, open an issue on the relevant extension repo (links on each repo's
GitHub page).

## Common questions

??? question "Do I need MIRAGE to use FlowPath?"
    No. FlowPath works with **any** source of QuPath detections that carry
    per-marker measurements — Cellpose or StarDist masks via AnnoMask, or any
    GeoJSON with marker measurements. MIRAGE is the reference upstream because its
    [measurement keys](usage.md#the-data-model) line up perfectly, but it isn't
    required.

??? question "What's the difference between the two ways to import cells?"
    On-ramp A imports MIRAGE's `cells.geojson` directly (already quantified).
    On-ramp B brings a label mask in via AnnoMask, which re-derives identical
    intensities in-app. Both end with detections that carry measurements. See
    [Usage → Step 1](usage.md#step-1-get-cells-into-qupath).

??? question "Which QuPath version do I need?"
    **QuPath 0.7.0 or later** — every FlowPath extension targets 0.7.0. See the
    [current versions](installation.md#current-versions).

??? question "Is FlowPath part of MIRAGE?"
    No. The FlowPath extensions are **independent, MIT-licensed QuPath
    extensions** by [`sceriff0`](https://github.com/sceriff0). MIRAGE is a
    separate Nextflow pipeline with its
    [own documentation](https://mirage-pipeline.readthedocs.io/).

??? question "Do GatingTree and qUMAP share the same cells?"
    Yes — both operate on the QuPath detections in the current image. Gate in
    GatingTree to assign PathClasses, then color the qUMAP by those same classes.

??? question "Can I reproduce or share a gating strategy?"
    Yes — GatingTree saves the full gate hierarchy to `flowpath.json`. Load it on
    another image to reproduce the gates (given the same markers are present).

??? question "How do I update the extensions?"
    Through the same catalog. When a new release is published, QuPath's extension
    manager offers the update.

## Installation { #installation }

??? failure "The catalog adds, but no extensions appear"
    Almost always a **QuPath version below 0.7.0**. Every FlowPath release pins a
    minimum of `v0.7.0`, so older QuPath won't show them. Check **Help → About**
    for your version and upgrade if needed.

??? failure "Adding the catalog URL fails or shows nothing"
    Check the URL for typos — it must be exactly the **raw** GitHub URL:
    ```
    https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
    ```
    (note `raw.githubusercontent.com`, not the repository web page). A
    network/proxy block on `githubusercontent.com` will also cause this.

??? failure "Install starts but the extension never loads"
    The release JAR may have failed to download. Try the
    [manual JAR install](installation.md#alternative-drop-in-a-jar) from the
    extension's Releases page, and restart QuPath fully.

??? failure "Extension menu entry is missing after install"
    Restart QuPath completely — extensions register at startup. If still missing,
    confirm the JAR landed in QuPath's extensions directory (Extensions → Manage
    extensions shows the path).

## GatingTree { #gatingtree }

??? failure "Gates show no cells / histograms are empty"
    Your detections probably have **no measurements**. Import a `cells.geojson`
    that carries intensities, or use AnnoMask with **intensity sampling enabled**.
    See [the data model](usage.md#the-data-model).

??? failure "I can't pick a compartment (Nucleus / Cytoplasm / Cell)"
    The data has no [per-compartment keys](usage.md#the-data-model) — GatingTree
    falls back to whole-cell / Mean. Re-export from MIRAGE with per-compartment
    quantification if you need them.

??? failure "Too many tiny/odd objects are being gated"
    Set the **quality filters** (area, eccentricity, solidity, perimeter, total
    intensity) before gating to drop segmentation artifacts.

## qUMAP { #qumap }

??? failure "Out-of-memory while computing UMAP"
    qUMAP estimates memory first and warns you. Switch subsampling to **Auto** or
    **Fixed**, or increase QuPath's max memory (Edit → Preferences) and restart.
    Large slides (>100K cells) should use subsampling + kNN projection — see the
    [performance table](usage.md#qumap).

??? failure "UMAP isn't colored by phenotype"
    Cells need **PathClasses** to color by. Gate them in GatingTree first, then
    recompute or recolor the embedding.

??? failure "Computation is very slow"
    Enable subsampling for large datasets and reduce epochs for a faster (coarser)
    embedding. See the [performance table](usage.md#qumap).

## AnnoMask { #annomask }

??? failure "Detections are misaligned with the image"
    A loaded mask is assumed to **align to the current image's pixel grid**. Make
    sure the mask resolution and orientation match the open OME-TIFF.

??? failure "Cell count differs from what I expected"
    AnnoMask produces **one detection per unique integer label**, merging multiple
    connected components of the same label — matching MIRAGE's count. Re-label the
    mask if you want components split.

??? failure "Measurements are empty after import"
    Enable **intensity sampling** in the AnnoMask dialog and make sure the
    OME-TIFF (with the channels) is the open image.
