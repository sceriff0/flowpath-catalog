# FlowPath Ecosystem Documentation Site — Design

**Date:** 2026-06-16
**Status:** Approved (brainstorming complete)
**Lives in:** `flowpath-catalog` repository

## Goal

Build a single ReadTheDocs (MkDocs Material) site that documents the **QuPath
side of the FlowPath ecosystem** — the `flowpath-catalog` hub plus the three
extensions (GatingTree, AnnoMask, qUMAP). The site mirrors the structure and
polish of the existing MIRAGE documentation site (`mirage.readthedocs.io`) but
uses a distinct visual identity. MIRAGE is **referenced and linked to**, not
re-documented, since it already owns its own site.

## Decisions (locked during brainstorming)

| Decision | Choice |
|---|---|
| MIRAGE scope | **Reference & link out** — document the QuPath side in depth; treat MIRAGE as the upstream producer with deep links to `mirage.readthedocs.io`. No duplication. |
| Site location | **Inside `flowpath-catalog`** — add `docs/`, `mkdocs.yml`, `.readthedocs.yaml` to the existing catalog repo (the conceptual hub of the suite). |
| Color palette | **Blue + Violet** (matches the logo gradient) — primary `#2563eb` (light `#3b82f6`, dark `#4338ca`), accent `#8b5cf6`. Hero gradient `blue-700 → violet-500`. Distinct from MIRAGE's teal/cyan. |
| Site title | **FlowPath** — tagline/`site_description`: *"FlowJo-style workflows for QuPath"* (from the logo). |
| Logo | User-supplied `flowpathlogo.png` (1774×887 blue→purple wordmark on black). Black background **knocked out to transparent** via PIL → `docs/assets/logo.png`; square **favicon** cropped from the magnifier icon → `docs/assets/favicon.png`. |
| Navigation model | **Approach C (hybrid)** — workflow-driven Getting Started + Walkthrough up front, per-component reference sections behind it. Same shape as MIRAGE's site. |
| Screenshots | **Placeholder image + caption** — a single neutral `placeholder.png` every `<img>` points to until replaced, each with a descriptive caption, plus a `screenshots/README.md` manifest listing the intended content of each shot. Pages render cleanly immediately. |

## Theme / Tech

- **MkDocs Material**, matching MIRAGE's feature set: `navigation.tabs`,
  `navigation.sections`, `navigation.indexes`, `navigation.top`, `toc.follow`,
  `search.*`, `content.code.copy`, `content.code.annotate`, `content.tabs.link`.
- **Plugins:** `search`, `include-markdown`, `glightbox` (image lightbox/zoom),
  `git-revision-date-localized`.
- **Markdown extensions:** same set as MIRAGE (admonition, attr_list, def_list,
  md_in_html, tables, pymdownx.* incl. superfences with mermaid, tabbed,
  tasklist, details, emoji).
- **Fonts:** Inter (text) / JetBrains Mono (code), same as MIRAGE.
- `extra_css: stylesheets/extra.css` — adapted from MIRAGE's, with the blue/orange
  palette variables and `.flowpath-hero` gradient.
- `exclude_docs` hides `superpowers/` and `assets/screenshots/README.md` from
  publishing.
- **`.readthedocs.yaml`:** ubuntu-22.04, python 3.11, `post_checkout: git fetch
  --unshallow || true` (git-revision-date needs full history), installs
  `docs/requirements.txt`.

## Navigation / Pages

| Section | Page | Purpose |
|---|---|---|
| **Home** | `index.md` | Hero block, "what is FlowPath", 4-component grid cards, ecosystem mermaid diagram. |
| **Getting Started** | `overview.md` | Concepts; where MIRAGE fits (high-level, links out); the two on-ramps. |
| | `installation.md` | Add the catalog; drop-in JAR; build from source. QuPath 0.7.0 / JDK 25. |
| | `walkthrough.md` | End-to-end story: MIRAGE output → import/AnnoMask → GatingTree → qUMAP. |
| **The Ecosystem** | `architecture.md` | The two-on-ramps data-flow mermaid; how the pieces connect. |
| | `measurement-keys.md` | The naming contract (bare marker + per-compartment keys) that makes output plug-and-play. |
| | `mirage.md` | What MIRAGE is, what it hands off, deep links to `mirage.readthedocs.io`. |
| **Extensions** | `gatingtree.md` | What/launch (`Ctrl+G`)/reads/writes (`flowpath.json`, `gate_pheno.csv`)/features table + screenshots. |
| | `annomask.md` | Mask→detections; two input modes; identical bincount to `quantify.py`; launch `Ctrl+Shift+M`. |
| | `qumap.md` | UMAP via SMILE; phenotype coloring; polygon tagging; launch `Ctrl+U`; `umap_coordinates.csv`. |
| **Catalog** | `catalog.md` | What the hub is; add-catalog URL; maintainer publish/release process. |
| **Help** | `troubleshooting.md` | Common issues (catalog not loading, missing measurements, OOM in qUMAP). |
| | `faq.md` | Short Q&A. |
| | `compatibility.md` | Version matrix: QuPath 0.7.0, JDK 25, current extension versions, MIRAGE. |
| | `citation.md` | How to cite each extension + QuPath + SMILE + UMAP + MIRAGE. |
| | `changelog.md` | Pointer to each extension's CHANGELOG / releases. |

## Accuracy contract (correcting MIRAGE's framing)

- All repo URLs are verified against `catalog.json` (the source of truth):
  - GatingTree → `github.com/sceriff0/qupath-extension-flowpath-gatingtree`
  - qUMAP → `github.com/sceriff0/qupath-extension-flowpath-qumap`
  - AnnoMask → `github.com/sceriff0/qupath-extension-annomask`
  - Catalog → `github.com/sceriff0/flowpath-catalog`
- Catalog add URL: `https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json`
- Current versions (from `catalog.json`): GatingTree v1.9.1, qUMAP v0.10.1, AnnoMask v0.3.4.
- Shortcuts: GatingTree `Ctrl+G`; AnnoMask `Ctrl+Shift+M` / `⌘⇧M`; qUMAP `Ctrl+U`.
- Facts sourced from each extension's README, not copied from possibly-stale text.

## Screenshot plan (~12 placeholders)

`docs/assets/screenshots/` with one `placeholder.png` and a `README.md` manifest.
Each spot uses `![caption](../assets/screenshots/placeholder.png){...}` so it
renders now and is swapped later.

1. Hero / banner (home)
2. QuPath → Manage extension catalogs → Add dialog (catalog/installation)
3. GatingTree — gate tree panel with multi-level hierarchy
4. GatingTree — 1D histogram with draggable threshold
5. GatingTree — 2D scatter / quadrant gate
6. GatingTree — cells recolored live in the viewer
7. AnnoMask — dialog (two input modes)
8. AnnoMask — before/after detections from a mask
9. qUMAP — embedding colored by phenotype + legend
10. qUMAP — polygon selection / UNFOCUSED grey-out
11. qUMAP — marker expression overlay (z-score / raw)
12. Walkthrough — end-to-end montage (optional)

## Out of scope

- Re-documenting MIRAGE's pipeline stages (MIRAGE site owns this).
- Auto-generating API/Javadoc for the extensions.
- CI to publish the site (ReadTheDocs auto-builds from the repo on push).
- Real screenshots (placeholders + manifest only; user supplies images later).
