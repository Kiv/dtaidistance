Clustering
----------

Agglomerative clustering
~~~~~~~~~~~~~~~~~~~~~~~~

A distance matrix can be used for time series clustering. You can use
existing methods such as ``scipy.cluster.hierarchy.linkage`` or one of
two included clustering methods (the latter is a wrapper for the SciPy
linkage method).

::

    # Custom Hierarchical clustering
    model1 = clustering.Hierarchical(dtw.distance_matrix_fast, {})
    # Keep track of full tree by using the HierarchicalTree wrapper class
    model2 = clustering.HierarchicalTree(model1)
    # You can also pass keyword arguments identical to instantiate a Hierarchical object
    model2 = clustering.HierarchicalTree(dists_fun=dtw.distance_matrix_fast, dists_options={})
    # SciPy linkage clustering
    model3 = clustering.LinkageTree(dtw.distance_matrix_fast, {})
    cluster_idx = model3.fit(series)

For models that keep track of the full clustering tree
(``HierarchicalTree`` or ``LinkageTree``), the tree can be visualised:

::

    model2.plot("hierarchy.png")

A number of options are also available to tune the layout of the figure. You can also pass your
own set of axes. The only assumption is that the tree is printed to ``ax[0]`` and the series to ``ax[1]``.

::

    fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(10, 10))
    show_ts_label = lambda idx: "ts-" + str(idx)
    model.plot("hierarchy.png", axes=ax, show_ts_label=show_ts_label,
               show_tr_label=True, ts_label_margin=-10,
               ts_left_margin=10, ts_sample_length=1)

.. figure:: https://people.cs.kuleuven.be/wannes.meert/dtw/hierarchy.png?v=2
   :alt: Clustering hierarchy


Active semi-supervised clustering
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The recommended method for perform active semi-supervised clustering using
DTAIDistance is to use the COBRAS for time series clustering: https://bitbucket.org/toon_vc/cobras_ts.
COBRAS is a library for semi-supervised time series clustering using pairwise constraints,
which natively supports both dtaidistance.dtw and kshape.
