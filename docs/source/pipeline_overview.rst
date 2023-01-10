Pipeline overview
===================

.. image:: images/Flowchart_16S_correct.svg
   :width: 1327
   :alt: Flowchart of the pipeline's processing steps


The pipeline is built using `Nextflow <https://www.nextflow.io/>`_ and processes data in the following steps:

* `FastQC <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ : Read quality control.
* `Cutadapt <https://cutadapt.readthedocs.io/en/stable/>`_ : Primer trimming.
* `DADA2 <https://benjjneb.github.io/dada2/index.html>`_ : Infer Amplicon Sequence Variants (ASVs) and taxonomic classification.
* `QIIME2 <https://qiime2.org/>`_ : Secondary analysis on diversity results.

    * Taxonomic classification: Taxonomical classification of ASVs.
    * Relative abundance tables: Exported relative abundance tables.
    * Alpha diversity rarefaction curves: Rarefaction curves for quality control.
    * Diversity analysis: High level overview with different diversity indices.
    
        * Alpha diversity indices: Diversity within samples
        * Beta diversity indices: Diversity between samples (e.g. PCoA plots)

With all the intermediate outputs from the steps above, the pipeline produces several reports aggregating the results in a comprehensible and elegant manner:

* `Krona plots <https://hpc.nih.gov/apps/kronatools.html>`_ : Interactive visualization of the diversity within samples.
* Sample reports: Aggregated report of all the relevant information for each sample (QC, ASVs present, etc).
* Analysis report: Aggregated report of all the samples, containing alpha and beta-diversity measures and a clustered heatmap.
* `MultiQC <https://multiqc.info/>`_ :Interactive aggregated report of all the quality control metrics.

Finally, the pipelines takes all the aforementioned outputs and produces a easily downloadable ZIP file.


