# Citation

If you use FlowPath in published work, please cite the **relevant extension(s)**
and the tools they build on. If you also used [MIRAGE](mirage.md) upstream,
please cite that too.

## FlowPath extensions

=== "GatingTree"

    > FlowPath: Interactive tree-based cell phenotyping for QuPath. (2026).
    > <https://github.com/sceriff0/qupath-extension-flowpath-gatingtree>

=== "qUMAP"

    > FlowPath - qUMAP: UMAP dimensionality reduction and visualization for
    > QuPath. (2026).
    > <https://github.com/sceriff0/qupath-extension-flowpath-qumap>

=== "AnnoMask"

    > FlowPath - AnnoMask: labeled-mask import for QuPath. (2026).
    > <https://github.com/sceriff0/qupath-extension-annomask>

## QuPath

All FlowPath extensions run on QuPath — please cite it:

> Bankhead, P. et al. (2017). QuPath: Open source software for digital pathology
> image analysis. *Scientific Reports*, 7, 16878.
> <https://doi.org/10.1038/s41598-017-17204-5>

## UMAP & SMILE (for qUMAP)

If you used [qUMAP](qumap.md), also cite the UMAP algorithm and the SMILE
library that implements it:

> McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform Manifold
> Approximation and Projection for Dimension Reduction. *arXiv:1802.03426*.
> <https://arxiv.org/abs/1802.03426>

> Haifeng Li. (2014). SMILE — Statistical Machine Intelligence and Learning
> Engine. <https://haifengl.github.io/>

## MIRAGE (upstream pipeline)

If you produced your inputs with MIRAGE, cite it as well — see MIRAGE's own
[citation page](https://mirage.readthedocs.io/) for the current reference.

!!! tip "Cite what you used"
    Only cite the extensions you actually used. A pure import-and-gate workflow
    needs GatingTree (+ AnnoMask if you imported a mask); add qUMAP and its
    references only if you computed an embedding.
