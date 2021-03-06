pySCENIC
========

|buildstatus|_ |pypipackage|_ |docstatus|_

pySCENIC is a lightning-fast python implementation of the SCENIC_ pipeline (Single-Cell rEgulatory Network Inference and
Clustering) which enables biologists to infer transcription factors, gene regulatory networks and cell types from
single-cell RNA-seq data.

The pioneering work was done in R and results were published in Nature Methods [1]_.
A new and comprehensive description of this Python implementation of the SCENIC pipeline is available in Nature Protocols [5]_ (`see here <https://doi.org/10.1038/s41596-020-0336-2>`_).

pySCENIC can be run on a single desktop machine but easily scales to multi-core clusters to analyze thousands of cells
in no time. The latter is achieved via the dask_ framework for distributed computing [2]_.

**Full documentation** is available on `Read the Docs <https://pyscenic.readthedocs.io/en/latest/>`_

News and releases
-----------------

0.11.0 | 2021-02-10
^^^^^^^^^^^^^^^^^^^

**Major features:**

* Updated arboreto_ release (GRN inference step) includes:

  * Support for sparse matrices (using the ``--sparse`` flag in ``pyscenic grn``, or passing a sparse matrix to ``grnboost2``/``genie3``).
  * Fixes to avoid dask metadata mismatch error

* Updated cisTarget:

  * Fix for metadata mismatch in ctx prune2df step
  * Support for databases Apache Parquet format
  * Faster loading from feather databases
  * Bugfix: loading genes from a database (previously missing the last gene name in the database)

* Support for Anndata input and output

* Package updates:

  * Upgrade to newer pandas version
  * Upgrade to newer numba version
  * Upgrade to newer versions of dask, distributed

* Input checks and more descriptive error messages.

  * Check that regulons loaded are not empty.

* Bugfixes:

  * In the regulons output from the cisTarget step, the gene weights were incorrectly assigned to their respective target genes (PR #254).
  * Motif url construction fixed when running ctx without pruning
  * Compression of intermediate files in the CLI steps
  * Handle loom files with non-standard gene/cell attribute names
  * Reformat the genesig gmt input/output
  * Fix AUCell output to loom with non-standard loom attributes


0.10.4 | 2020-11-24
^^^^^^^^^^^^^^^^^^^

* Included new CLI option to add correlation information to the GRN adjacencies file. This can be called with ``pyscenic add_cor``.



See also the extended `Release Notes <https://pyscenic.readthedocs.io/en/latest/releasenotes.html>`_.

Overview
--------

The pipeline has three steps:

1. First transcription factors (TFs) and their target genes, together defining a regulon, are derived using gene inference methods which solely rely on correlations between expression of genes across cells. The arboreto_ package is used for this step.
2. These regulons are refined by pruning targets that do not have an enrichment for a corresponding motif of the TF effectively separating direct from indirect targets based on the presence of cis-regulatory footprints.
3. Finally, the original cells are differentiated and clustered on the activity of these discovered regulons.

The most impactful speed improvement is introduced by the arboreto_ package in step 1. This package provides an alternative to GENIE3 [3]_ called GRNBoost2. This package can be controlled from within pySCENIC.


All the functionality of the original R implementation is available and in addition:

1. You can leverage multi-core and multi-node clusters using dask_ and its distributed_ scheduler.
2. We implemented a version of the recovery of input genes that takes into account weights associated with these genes.
3. Regulons, i.e. the regulatory network that connects a TF with its target genes, with targets that are repressed are now also derived and used for cell enrichment analysis.


Additional resources
--------------------

For more information, please visit LCB_, or SCENIC_ (R version).
The CLI to pySCENIC has also been streamlined into a pipeline that can be run with a single command, using the Nextflow workflow manager.
There are two Nextflow implementations available:

* `SCENICprotocol`_: A Nextflow DSL1 implementation of pySCENIC alongside a basic "best practices" expression analysis. Includes details on pySCENIC installation, usage, and downstream analysis, along with detailed tutorials.
* `VSNPipelines`_: A Nextflow DSL2 implementation of pySCENIC with a comprehensive and customizable pipeline for expression analysis. Includes additional pySCENIC features (multi-runs, integrated motif- and track-based regulon pruning, loom file generation).


Acknowledgments
---------------

We are grateful to all providers of TF-annotated position weight matrices, in particular Martha Bulyk (UNIPROBE), Wyeth Wasserman and Albin Sandelin (JASPAR), BioBase (TRANSFAC), Scot Wolfe and Michael Brodsky (FlyFactorSurvey) and Timothy Hughes (cisBP).


References
----------

.. [1] Aibar, S. et al. SCENIC: single-cell regulatory network inference and clustering. Nat Meth 14, 1083–1086 (2017).
.. [2] Rocklin, M. Dask: parallel computation with blocked algorithms and task scheduling. conference.scipy.org
.. [3] Huynh-Thu, V. A. et al. Inferring regulatory networks from expression data using tree-based methods. PLoS ONE 5, (2010).
.. [4] Zeisel, A. et al. Cell types in the mouse cortex and hippocampus revealed by single-cell RNA-seq. Science 347, 1138–1142 (2015).
.. [5] Van de Sande B., Flerin C., et al. A scalable SCENIC workflow for single-cell gene regulatory network analysis. Nat Protoc. June 2020:1-30. doi:10.1038/s41596-020-0336-2

.. |buildstatus| image:: https://travis-ci.org/aertslab/pySCENIC.svg?branch=master
.. _buildstatus: https://travis-ci.org/aertslab/pySCENIC

.. |pypipackage| image:: https://img.shields.io/pypi/v/pySCENIC?color=%23026aab
.. _pypipackage: https://pypi.org/project/pyscenic/

.. |docstatus| image:: https://readthedocs.org/projects/pyscenic/badge/?version=latest
.. _docstatus: http://pyscenic.readthedocs.io/en/latest/?badge=latest

.. _SCENIC: http://scenic.aertslab.org
.. _dask: https://dask.pydata.org/en/latest/
.. _distributed: https://distributed.readthedocs.io/en/latest/
.. _arboreto: https://arboreto.readthedocs.io
.. _LCB: https://aertslab.org
.. _`SCENICprotocol`: https://github.com/aertslab/SCENICprotocol
.. _`VSNPipelines`: https://github.com/vib-singlecell-nf/vsn-pipelines
.. _notebooks: https://github.com/aertslab/pySCENIC/tree/master/notebooks
.. _issue: https://github.com/aertslab/pySCENIC/issues/new
.. _PyPI: https://pypi.python.org/pypi/pyscenic

