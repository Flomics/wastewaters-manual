FAQ
###

Here are some answers to frequently-asked questions.
Got a question that isn't answered here? You can `write <mailto:support@flomics.com>`_ to us and we will answer as fast as we can!

.. contents::
    :local:
    :depth: 2



Why …
===============

.. _heatscale:

…does the scale of the heatmap go from -2 to 2?
-------------------------------------------

In order to effectively show the difference in abundances between the top 30 most abundant features among the samples, we carry out a mathematical transformation of the feature's abundance, to end up with the decimal logarithm of the abundance in percent. First we transform the abundance to a percent scale. Since the decimal logarithm of zero is undefined, whe add a pseudocount of 0.01 to those features that have an abundance of 0%.
Thus, the normalized abundance for a sample that has an abundance of 0% will be -2, and will be 2 for a sample that has an abundance of 100%.


.. _heattrail:

…do the names of the features in the heatmap have several semicolons on them (";;;")?
--------------------------------------------------------------------------------------

This is because the pipeline follows the ``kpcofgs`` (Kingdom;Phylum;Class;Order;Family;Genus;Species) convention for naming the ASVs found in the sample. Although the pipeline will try to classify each ASVs to the deepest level, this won't always be possible, leaving an ASV classified to, for example, ``family`` level at most. Having two semicolons at the end of the feature name (for example: ``Bacteria;Bacteria;Firmicutes;Bacilli;Lactobacillales;Streptococcaceae;Streptoccocus;;1``) means that the pipeline has classified with confidence that this ASV belongs to the Family ``Streptococcus``, but can't assign with confidence its Genus and Species. The number at the end of the feature name is the confidence of the assignment, ranging from 1 to 0.
