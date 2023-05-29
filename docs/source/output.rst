Output
========

The directories and output files listed below can be found inside the the zip file. All paths are relative to the top-level **results** directory.

FastQC
++++++++++++++

FastQC (ver. 0.11.9) gives general quality metrics about your reads. It provides information about the quality score distribution across your reads and per base sequence content (%T/A/G/C). You get information about adapter contamination and other overrepresented sequences.

For further reading and documentation see the `FastQC help <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/>`_.

The FastQC plots displayed in the MultiQC report shows untrimmed reads. They may contain adapter sequences and potentially regions with low quality. 

**Output directory:  <RESULTS>/<SAMPLE>/1-fastqc/**

    ``<SAMPLE>_fastqc.html``
        FastQC report, containing quality metrics for your untrimmed raw fastq files
    ``zips/<SAMPLE>_fastqc.zip``
        zip file containing the FastQC report, tab-delimited data file and plot images

Fastp
++++++++++++++
The Flomics/SARSCoV2 pipeline uses `Fastp <https://github.com/OpenGene/fastp/>`_ for removal of adapter contamination and trimming of low quality regions. Fastp runs FastQC after it finishes.

MultiQC reports the number of reads that pass the filter of Fastp in the General Statistics table, along with the reads that didn't pass the filter and the reason.

**Output directory: <RESULTS>/<SAMPLE>/2-fastp/**

    ``<SAMPLE>.fastp.html``
        Fastp report, containing all the information of reads trimming.
    ``fastqc/zips/<SAMPLE>_fastqc.zip``
        Zip file containing the FastQC report after trimming.
    ``fastqc/<SAMPLE>_trim_fastqc.html``
        FastQC report, containing quality metrics for your trimmed reads.
    ``logs/<SAMPLE>.fastqc.log``
        Log files for fastp.

Kraken2
++++++++++++++

`Kraken2 <https://ccb.jhu.edu/software/kraken2/index.shtml?t=manual>`_ is a sequence classifier that assigns taxonomic labels to DNA sequences. Kraken examines the k-mers within a query sequence and uses the information within those k-mers to query a database. That database maps k-mers to the lowest common ancestor (LCA) of all genomes known to contain a given k-mer.

We used a Kraken2 database in this workflow to classify reads in their species so that it is possible to identify reads for SARSCoV2 and determine whether it is a positive or negative sample. This pipeline filters kraken2 reports to obtain information about unmapped reads and those that come from species. It creates piecharts and tables summarizing those results at species level. Piecharts include the top10 most abundant species, human and unclassified. If there are more species they will be classified as Other. In case that SARSCoV2 is present in the sample but not in the Top10, it will also include it in the piechart.

**Output directory: <RESULTS>/<SAMPLE>/3-kraken2/**

    ``figures/<SAMPLE>.piechart.png``
        Per sample, shows the composition at species level of virus and human in the sample (for the top10 most abundant viruses)
    ``tables/<SAMPLE>.table.png``
        Per samples, includes the composition at species of virus and human in the sample (all virus present)
    ``reports/<SAMPLE>.kraken2.report.png``
        Kraken2 report of classified and unclassified reads


Bowtie 2
++++++++++++++
`Bowtie 2 <https://bio-bwa.sourceforge.net/>`_ is an ultrafast and memory-efficient tool for aligning sequencing reads to long reference sequences. Bowtie 2 supports gapped, local, and paired-end alignment modes.

**Output directory: <RESULTS>/<SAMPLE>/4-bam/**

    ``logs/<SAMPLE>.bowtie2.log/``
        Bowtie 2 mapping log file.

SAMtools
++++++++++++++
Bowtie 2 BAM files are further processed with `SAMtools <https://samtools.sourceforge.net/>`_ to coordinate sort and index the alignments, as well as to generate read mapping statistics.

**Output directory: <RESULTS>/<SAMPLE>/4-bam/**

    ``bam/<SAMPLE>.sorted.bam``
        Original BAM sorted file created by Bowtie 2.
    ``bai/<SAMPLE>.sorted.bam.bai``
        Indexed bam sorted files.
    ``samtools_stats/<SAMPLE>.sorted.bam.flagstat, samtools_stats/<SAMPLE>.sorted.bam.idxstats and samtools_stats/<SAMPLE>.sorted.bam.stats``
        Files generated from the alignment files.

Qualimap
++++++++++++++++
`Qualimap <http://qualimap.conesalab.org/>`_ is a standalone package written in java. It calculates read alignment assignment, transcript coverage, read genomic origin, junction analysis and 3'-5' bias.

**Output directory: <RESULTS>/<SAMPLE>/5-qualimap/<SAMPLE>.sorted_stats**

iVar trim
++++++++++++++++++
`iVar <https://andersen-lab.github.io/ivar/html/>`_ is used to trim amplicon primer sequences from the aligned reads.

**Output directory: <RESULTS>/<SAMPLE>/4-bam/**

    ``bam/<SAMPLE>.sorted.trim.bam``
        Original BAM sorted file created by Bowtie 2.
    ``bai/<SAMPLE>.trim.sorted.bam.bai``
        Indexed bam sorted files.

iVar variants
++++++++++++++++
iVar is used again to do variant calling.

**Output directory: <RESULTS>/<SAMPLE>/5-variants/**

    ``<SAMPLE>.tsv``
        Variants in TSV format that PASS all filters.
    ``<SAMPLE>.modified.tsv``
        TSV file containing additional information regarding the variants.
    ``<SAMPLE>.pass.vcf.gz``
        Variants in TSV format that PASS all filters, compliant with the VCF file specifications.
    ``<SAMPLE>.pass.vcf.gz.tbi``
        Variants in TSV format that PASS all filters, compliant with the VCF file specifications.
    ``<SAMPLE>.all.vcf.gz``
        Variants in TSV format that PASS and FAIL all filters ,compliant with the VCF file specifications. Contains the variants that have a lower allelic frequency than the threshold.
    ``logs/<SAMPLE>.variant.counts.log``
        Variant counts for variants that PASSED all filters.
    ``bcftools_stats/<SAMPLE>.pass.bcftools_stats.txt``
        Statistics and counts obtained from low frequency variants VCF file.

SnpEff and SnpSift
++++++++++++++++++++
`SnpEff <https://pcingola.github.io/SnpEff/>`_ is a genetic variant annotation and functional effect prediction toolbox. It annotates and predicts the effects of genetic variants on genes and proteins (such as amino acid changes).

`SnpSift <https://pcingola.github.io/SnpEff/ss_introduction/>`_ annotates genomic variants using databases, filters, and manipulates genomic annotated variants. After annotation with SnpEff, you can use SnpSift to help filter large genomic datasets in order to find the most significant variants.

**Output directory: <RESULTS>/<SAMPLE>/8-snpeff/**

    ``<SAMPLE>.snpEff.csv``
        Variant annotation csv file.
    ``<SAMPLE>.snpEff.genes.txt``
        Gene table for annotated variants.
    ``<SAMPLE>.snpEff.summary.html``
        Summary html file for variants.
    ``<SAMPLE>.snpEff.vcf.gz``
        VCF file with variant annotations.
    ``<SAMPLE>.snpEff.vcf.gz.tbi``
        Index for VCF file with variant annotations.
    ``<SAMPLE>.snpSift.table.modified.txt``
        SnpSift summary table, with additional information.
    ``<SAMPLE>.snpSift.table.txt``
        SnpSift summary table.
    ``<SAMPLE>_mutations.tsv``
        TSV table containing the sample mutations that appear in our curated mutation database as mutations of interest or concern.

Freyja
++++++++++++++++
`Freyja <https://github.com/andersen-lab/Freyja>`_ is a tool to recover relative lineage abundances from mixed SARS-CoV-2 samples from a sequencing dataset (BAM aligned to the Hu-1 reference). The method uses lineage-determining mutational "barcodes" derived from the UShER global phylogenetic tree as a basis set to solve the constrained (unit sum, non-negative) de-mixing problem.

**Output directory: <RESULTS>/<SAMPLE>/5-variants/**

    ``<SAMPLE>_demix``
        File containing the relative abundances of all the SARS-CoV-2 lineages found in the sample.

Markdown report
++++++++++++++++++
We use markdown to generate a summary report for each sample.

**Output directory: <RESULTS>/<SAMPLE>/9-report/**

    ``<SAMPLE>_report.html``
        Report with information about all steps per sample. It provides information about number of sequences, sequences trimmed, sequences mapped, coverage, variants detected, variants interpretation.

MultiQC
++++++++++++++
MultiQC (ver. 1.10.1) is a visualization tool that generates a single HTML report summarizing all QC information for all the samples in your project.

The pipeline has special steps which allow the software versions used to be reported in the MultiQC output for future traceability.

**Output directory: <RESULTS>/multiqc/**

    ``general.report.html``
        MultiQC report - a standalone HTML file that can be viewed in a web browser.

Pipeline information
++++++++++++++++++++++
The pipeline also provides a table listing software used and their respective versions.

**Output directory: <RESULTS>/pipeline_info/**

    ``software_versions.tsv``