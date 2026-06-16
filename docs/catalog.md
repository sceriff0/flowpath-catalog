# The FlowPath Catalog

All three FlowPath extensions ship through a single QuPath **extension
catalog** — `catalog.json`, served straight from GitHub. Add its URL once and
QuPath will offer to install, and later update, every extension from one place.

!!! tip "Add the catalog"
    In QuPath: **Extensions → Manage extensions → Manage extension catalogs →
    Add catalog →**

    ```
    https://raw.githubusercontent.com/sceriff0/flowpath-catalog/main/catalog.json
    ```

    Repo: <https://github.com/sceriff0/flowpath-catalog>

<figure class="screenshot" markdown>
![FlowPath extensions in QuPath's extension manager](assets/screenshots/placeholder.png){ .glightbox }
<figcaption>The catalog populates QuPath's extension manager with the FlowPath suite. <em>(placeholder)</em></figcaption>
</figure>

## What the catalog is (and isn't)

The catalog is the suite's **distribution hub** — QuPath's mechanism for
discovering extensions and their releases. It is **not** part of the data flow:
it never touches your images or cells. It only tells QuPath:

- which extensions exist (GatingTree, qUMAP, AnnoMask),
- where to download each release JAR, and
- the minimum QuPath version each release needs.

## What's inside `catalog.json`

A single JSON document with a `name`, a `description`, and an `extensions`
array. Each extension lists its releases, newest first:

```json
{
  "name": "FlowPath - GatingTree",
  "description": "Interactive tree-based cell phenotyping with hierarchical gates.",
  "author": "sceriff0",
  "homepage": "https://github.com/sceriff0/qupath-extension-flowpath-gatingtree",
  "releases": [
    {
      "name": "v1.9.1",
      "main_url": "https://github.com/sceriff0/qupath-extension-flowpath-gatingtree/releases/download/v1.9.1/FlowPath.-.GatingTree-1.9.1.jar",
      "version_range": { "min": "v0.7.0" }
    }
  ]
}
```

The three extensions and their homepages:

| Extension | Homepage |
|---|---|
| FlowPath - GatingTree | <https://github.com/sceriff0/qupath-extension-flowpath-gatingtree> |
| FlowPath - qUMAP | <https://github.com/sceriff0/qupath-extension-flowpath-qumap> |
| FlowPath - AnnoMask | <https://github.com/sceriff0/qupath-extension-annomask> |

Every release pins `version_range.min` to `v0.7.0` — the minimum QuPath version
the suite supports. See the [compatibility matrix](compatibility.md) for the
current versions.

## For maintainers: publishing a new release

When you cut a new version of an extension:

1. **Build and release the JAR** on the extension's own GitHub repo (a tagged
   GitHub Release with the JAR as an asset).
2. **Add a release entry** to that extension's `releases` array in
   `catalog.json`, newest first:
   ```json
   {
     "name": "vX.Y.Z",
     "main_url": "https://github.com/sceriff0/<repo>/releases/download/vX.Y.Z/<asset>.jar",
     "version_range": { "min": "v0.7.0" }
   }
   ```
3. **Match the `main_url` to the actual asset name** — the asset filename must
   exist at that URL (QuPath downloads it verbatim).
4. **Commit and push** to `main`. Because QuPath reads the raw `main` URL, the
   new release is available to users as soon as it's merged — no separate
   publish step.

!!! warning "The asset URL must resolve"
    QuPath downloads `main_url` directly. A typo, a missing release asset, or a
    private repo will make the install fail silently. Paste the `main_url` into
    a browser to confirm it downloads before committing.

!!! note "Keep older releases listed"
    Leave previous release entries in place. QuPath uses `version_range` to
    offer users the newest release compatible with their QuPath version.
