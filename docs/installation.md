# Installation

All three FlowPath extensions install through a single QuPath **extension
catalog** — think of it as FlowPath's app store. Add one URL and QuPath will
offer to install, and later update, every extension for you.

!!! info "Requirements"
    - **[QuPath](https://qupath.github.io/) 0.7.0 or later** — every FlowPath
      extension targets QuPath 0.7.0 and runs on Windows, macOS, or Linux.
    - Java is bundled with QuPath at runtime. Building from source additionally
      needs **JDK 25**.

## Recommended: add the catalog

In QuPath:

**Extensions → Manage extensions → Manage extension catalogs → Add catalog →**

```
https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
```

Then, back in **Manage extensions**, install **GatingTree**, **AnnoMask**, and
**qUMAP** with the `+` button. Restart QuPath when prompted.

<figure class="screenshot" markdown>
![Adding the FlowPath catalog URL in QuPath](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>QuPath → Manage extension catalogs → Add, with the FlowPath catalog URL pasted in. <em>(placeholder)</em></figcaption>
</figure>

!!! tip "What the catalog is"
    The catalog is just `catalog.json` served from GitHub. It tells QuPath which
    extensions exist, where to download each release JAR, and the minimum QuPath
    version each needs — it never touches your images or cells. New releases are
    added to it as they ship, so QuPath always offers the newest version
    compatible with your QuPath.

## Verify the install

After restarting QuPath, open the **Extensions** menu. You should see:

| Extension | Menu entry | Shortcut |
|---|---|---|
| GatingTree | Extensions → FlowPath - GatingTree | ++ctrl+g++ |
| AnnoMask | Extensions → FlowPath - AnnoMask | ++ctrl+shift+m++ (++cmd+shift+m++ on macOS) |
| qUMAP | Extensions → FlowPath - qUMAP | ++ctrl+u++ |

If they're there, you're ready for the [walkthrough in Usage](usage.md).

!!! warning "Catalog adds, but no extensions appear?"
    Almost always a QuPath version below 0.7.0, or a typo in the catalog URL (it
    must be the **raw** `raw.githubusercontent.com` URL). See
    [Troubleshooting](troubleshooting.md#installation).

## Current versions

The latest releases published in
[`catalog.json`](https://github.com/sceriff0/flowpath-catalog/blob/main/catalog.json)
— the source of truth. QuPath's extension manager reads it live.

| Extension | Latest | Minimum QuPath |
|---|---|---|
| FlowPath - GatingTree | **v1.9.2** | v0.7.0 |
| FlowPath - qUMAP | **v0.10.2** | v0.7.0 |
| FlowPath - AnnoMask | **v0.3.5** | v0.7.0 |

Per-extension changelogs and release history live on each repo's Releases page
(linked below).

## Alternative: drop in a JAR

Prefer to do it by hand? Download the release `.jar` from the extension's GitHub
Releases page and drop it into QuPath's **extensions directory** (Extensions →
Manage extensions shows the path), then restart QuPath.

| Extension | Releases |
|---|---|
| GatingTree | <https://github.com/sceriff0/qupath-extension-flowpath/releases> |
| AnnoMask | <https://github.com/sceriff0/qupath-extension-annomask/releases> |
| qUMAP | <https://github.com/sceriff0/qupath-extension-flowpath-qumap/releases> |

## Alternative: build from source

Each extension uses the standard `qupath-extension-settings` Gradle plugin and
needs **JDK 25** plus QuPath 0.7.0 artefacts. The built JAR lands in
`build/libs/` — drag it onto QuPath.

=== "GatingTree"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-flowpath.git
    cd qupath-extension-flowpath
    ./gradlew build
    ```

=== "AnnoMask"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-annomask.git
    cd qupath-extension-annomask
    ./gradlew build
    ```

=== "qUMAP"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-flowpath-qumap.git
    cd qupath-extension-flowpath-qumap
    ./gradlew build
    ```
