Pipeline overview
===================

.. .. image:: images/Flowchart_16S_correct.svg
..     :width: 1327
..     :alt: Flowchart of the pipeline's processing steps


The pipeline is built using `Nextflow <https://www.nextflow.io/>`_ and processes data in the following steps:

* `FastQC <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ : Read quality control.
* `fastp <https://academic.oup.com/bioinformatics/article/34/17/i884/5093234?login=false>`_ : Adapter trimming.
* `Kraken2 <https://ccb.jhu.edu/software/kraken2/>`_ : Taxonomic classification.
* `Bowtie 2 https://bowtie-bio.sourceforge.net/bowtie2/index.shtml`_ : Read alignment versus the SARS-CoV-2 Wuhan genome.
* `SAMtools <https://sourceforge.net/projects/samtools/files/samtools/>`_ : Sort and index alignments.
* `iVar trim <https://github.com/andersen-lab/ivar>`_ : Primer sequence removal.
* `Freyja <https://github.com/andersen-lab/Freyja>`_ : Recovering of relative lineage abundances.
* `Qualimap <http://qualimap.conesalab.org/>`_ : Quality control of alignment.
* `iVar variants <https://github.com/andersen-lab/ivar>`_ : Variant calling.
* `SnpEff and SnpSift <https://pcingola.github.io/SnpEff/>`_ : Variant annotation.

With all the intermediate outputs from the steps above, the pipeline produces several reports aggregating the results in a comprehensible and elegant manner:

* Sample reports: Aggregated report of all the relevant information for each sample (QC, lineages present, etc).
* `MultiQC <https://multiqc.info/>`_ : Interactive aggregated report of all the quality control metrics.

Finally, the pipeline takes all the aforementioned outputs and produces a easily downloadable ZIP file.


