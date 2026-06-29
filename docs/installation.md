# Installation

All three FlowPath extensions install through a single QuPath **extension
catalog** — think of it as FlowPath's app store. Add one URL and QuPath will
offer to install, and later update, every extension for you.

!!! info "Requirements"
    - **[QuPath](https://qupath.github.io/) 0.7.0 or later** — every FlowPath
      extension targets QuPath 0.7.0.
    - Building from source additionally needs **JDK 25**.
    - See the [compatibility matrix](compatibility.md) for current versions.

## Recommended: add the catalog

In QuPath:

**Extensions → Manage extensions → Manage extension catalogs → Add catalog →**

```
https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
```

<figure class="screenshot" markdown>
![Adding the FlowPath catalog URL in QuPath](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>QuPath → Manage extension catalogs → Add, with the FlowPath catalog URL pasted in. <em>(placeholder)</em></figcaption>
</figure>

Then, back in **Manage extensions**, install **GatingTree**, **AnnoMask**, and
**qUMAP** with the `+` button. Restart QuPath when prompted.

<figure class="screenshot" markdown>
![The FlowPath extensions listed in QuPath's extension manager](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>The three FlowPath extensions available to install from the catalog. <em>(placeholder)</em></figcaption>
</figure>

!!! tip "The catalog is the distribution hub"
    The catalog itself is *not* part of the data flow — it only tells QuPath
    where to download the extension JARs and how to check for updates. See
    [The FlowPath Catalog](catalog.md) for what's inside it.

## Alternative: drop in a JAR

Prefer to do it by hand? Download the release `.jar` for the extension you want
from its GitHub Releases page and drop it into QuPath's **extensions
directory** (Extensions → Manage extensions shows the path), then restart
QuPath.

| Extension | Releases |
|---|---|
| GatingTree | <https://github.com/sceriff0/qupath-extension-flowpath-gatingtree/releases> |
| AnnoMask | <https://github.com/sceriff0/qupath-extension-annomask/releases> |
| qUMAP | <https://github.com/sceriff0/qupath-extension-flowpath-qumap/releases> |

## Alternative: build from source

Each extension uses the standard `qupath-extension-settings` Gradle plugin and
requires **JDK 25** and QuPath 0.7.0 artefacts.

=== "GatingTree"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-flowpath-gatingtree.git
    cd qupath-extension-flowpath-gatingtree
    ./gradlew build
    # JAR lands in build/libs/ → drag onto QuPath
    ```

=== "AnnoMask"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-annomask.git
    cd qupath-extension-annomask
    ./gradlew build
    # JAR lands in build/libs/ → drag onto QuPath
    ```

=== "qUMAP"

    ```bash
    git clone https://github.com/sceriff0/qupath-extension-flowpath-qumap.git
    cd qupath-extension-flowpath-qumap
    ./gradlew build
    # JAR lands in build/libs/ → drag onto QuPath
    ```

## Verify the install

After restarting QuPath, open the **Extensions** menu. You should see:

| Extension | Menu entry | Shortcut |
|---|---|---|
| GatingTree | Extensions → FlowPath - GatingTree | ++ctrl+g++ |
| AnnoMask | Extensions → FlowPath - AnnoMask | ++ctrl+shift+m++ (++cmd+shift+m++ on macOS) |
| qUMAP | Extensions → FlowPath - qUMAP | ++ctrl+u++ |

If they're there, you're ready for the [walkthrough](walkthrough.md).

!!! warning "Catalog not showing extensions?"
    See [Troubleshooting](troubleshooting.md#catalog) — it's almost always a
    QuPath version below 0.7.0 or a typo in the catalog URL.
