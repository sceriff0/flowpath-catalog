# FAQ

??? question "Do I need MIRAGE to use FlowPath?"
    No. FlowPath works with **any** source of QuPath detections that carry
    per-marker measurements — Cellpose or StarDist masks via
    [AnnoMask](annomask.md), or any GeoJSON with marker measurements. MIRAGE is
    the reference upstream because its [measurement keys](measurement-keys.md)
    line up perfectly, but it isn't required.

??? question "What's the difference between the two ways to import cells?"
    [On-ramp A](architecture.md) imports MIRAGE's `cells.geojson` directly (it's
    already quantified). [On-ramp B](architecture.md) brings a **label mask**
    into QuPath via [AnnoMask](annomask.md), which re-derives identical
    intensities in-app. Both end with detections that carry measurements.

??? question "Which QuPath version do I need?"
    **QuPath 0.7.0 or later** — every FlowPath extension targets 0.7.0. See
    [Compatibility](compatibility.md).

??? question "Is FlowPath part of MIRAGE?"
    No. The FlowPath extensions are **independent, MIT-licensed QuPath
    extensions** by [`sceriff0`](https://github.com/sceriff0). MIRAGE is a
    separate Nextflow pipeline with its
    [own documentation](https://mirage.readthedocs.io/).

??? question "Do GatingTree and qUMAP share the same cells?"
    Yes. Both operate on the **QuPath detections** in the current image. Gate in
    GatingTree to assign PathClasses, then color the qUMAP by those same
    classes — they're the same objects.

??? question "Can I reproduce or share a gating strategy?"
    Yes — GatingTree saves the full gate hierarchy to `flowpath.json`. Load it
    on another image to reproduce the gates (subject to the same markers being
    present).

??? question "What are 'per-compartment' measurements?"
    Keys of the form `"<MARKER>: <Compartment>: <Statistic>"`, e.g.
    `CD3: Nucleus: Mean`, for Nucleus / Cytoplasm / Cell and Mean / Median /
    Sum. FlowPath reads them natively and falls back to whole-cell / Mean if
    they're absent. See [Measurement Keys](measurement-keys.md).

??? question "How do I update the extensions?"
    Through the same catalog. When a new release is published, QuPath's
    extension manager offers the update. See [The Catalog](catalog.md).

??? question "qUMAP runs out of memory on a big slide — what do I do?"
    Use subsampling (Auto or Fixed) and/or raise QuPath's memory limit. See the
    [qUMAP performance notes](qumap.md#performance) and
    [Troubleshooting](troubleshooting.md#qumap).

??? question "Which extension does what, again?"
    [AnnoMask](annomask.md) imports masks → detections. [GatingTree](gatingtree.md)
    gates them into named phenotypes. [qUMAP](qumap.md) embeds them in 2D for
    exploration. The [catalog](catalog.md) installs all three.
