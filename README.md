# FlowPath — QuPath Extension Suite

[![QuPath](https://img.shields.io/badge/QuPath-%E2%89%A50.7.0-2563eb.svg)](https://qupath.github.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-8b5cf6.svg)](https://opensource.org/licenses/MIT)
[![Docs](https://img.shields.io/badge/docs-flowpath.readthedocs.io-success.svg)](https://flowpath.readthedocs.io/)

FlowJo-style workflows for [QuPath](https://qupath.github.io/): a suite of three
extensions for **single-cell analysis of multiplexed tissue imaging** (CODEX,
MIBI, mIF). Import cells, gate them into named phenotypes, and explore them in a
UMAP — all inside QuPath. Picks up where the
[MIRAGE](https://mirage-pipeline.readthedocs.io/) pipeline leaves off.

| Extension | What it does |
|---|---|
| [GatingTree](https://github.com/sceriff0/qupath-extension-flowpath-gatingtree) | Interactive tree-based cell phenotyping with hierarchical gates. |
| [qUMAP](https://github.com/sceriff0/qupath-extension-flowpath-qumap) | UMAP dimensionality reduction and visualization. |
| [AnnoMask](https://github.com/sceriff0/qupath-extension-annomask) | Import labeled segmentation masks (MIRAGE, Cellpose, StarDist…) as QuPath detections. |

This repo is the **catalog** (`catalog.json`) — QuPath's one-URL install hub for
all three.

## Install

In QuPath: **Extensions → Manage extensions → Manage extension catalogs → Add
catalog**, then paste:

```
https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
```

Install GatingTree, AnnoMask, and qUMAP with `+`, then restart QuPath.

## 📖 Documentation

Full docs — install, usage, options, troubleshooting, citations — live at
**<https://flowpath.readthedocs.io/>**.

## License

MIT. The FlowPath extensions are independent, MIT-licensed QuPath extensions by
[`sceriff0`](https://github.com/sceriff0).
