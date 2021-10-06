# Weighted Nearest Neighbors Analysis implemented in Python (pyWNN)

This is a port of [SEURAT's weighted nearest neighbors analysis](#https://satijalab.org/seurat/reference/findmultimodalneighbors) described in [this publication](#https://www.sciencedirect.com/science/article/pii/S0092867421005833)

This requires as input a [Scanpy](#https://scanpy.readthedocs.io/en/stable/index.html) AnnData object with representations of the two modalities stored in the obsm attribute like so:

```
adata.obsm['RNA_PCA'] # PCA of the RNA modality
adata.obsm['ADT_PCA'] # PCA of the Antibody-Derived Tags protein modality from CITE-Seq
```

You can call the representations whatever you like as they will be passed as an argument to pyWNN.

You then run pyWNN like so:

```
from pyWNN import pyWNN
WNNobj = pyWNN(adata, reps=['RNA_PCA', 'ADT_PCA'], npcs=[30,18], n_neighbors=20, seed=14)
adata = WNNobj.compute_wnn(adata_py)
```

The .uns and .obsp attributes of adata will be updated to included a new weighted nearest neighbor graph that smartly weights the 2 modalities. You can subsequently run UMAP with this new weighted nearest neighbor graph like so:

```
sc.tl.umap(adata, neighbors_key='WNN')
```

For validation of the method, you can provide distance matrices to pyWNN using the `distance` argument. This is a list of 4 sparse csr_matrices containing KNN distance matrices such as `[RNA_PCA_K20, ADT_PCA_K20, RNA_PCA_K200, ADT_PCA_K200]` reflecting modality 1 with 20 nearest neighbors, modality 2 with 20 nearest neighbors, modality 1 with 200 nearest neighbors, modality 2 with 200 nearest neighbors.

See the notebook in the Tutorials directory for running this on the [initial SEURAT example data](#https://satijalab.org/seurat/articles/weighted_nearest_neighbor_analysis.html).

