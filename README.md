# Bayesian-classifiers
Description: Naïve Bayesian classifiers for taxonomic classification of amplicon sequences in QIIME 2.

# 16S rRNA gene V3 region using 341F/534R primers (Muyzer et al. 1993)
As recommended by Werner et al. (2012), taxonomic classification of 16S rRNA genes may be more accurate using naïve Bayesian classifiers that have been trained on the amplified region of the 16S rRNA gene. Reference sequences and 7-level consensus taxonomic annotation from SILVA (Quast et al., 2012; Yilmaz et al. 2013) are provided that have been trimmed to the V3 region (~170 to 200 bp) via amplification by primers 341F and 534R (Muyzer et al. 1993). These primers were first used for high-throughput, paired-end Illumina sequencing of PCR amplicons by Bartram et al. (2011), who used identical primers with different nomenclature (i.e., 341F and 518R).

QIIME-compatible reference files for the entire 16S rRNA gene (99_otus_16S.fasta and consensus_taxonomy_7_levels_99.txt) were imported as artifact files (.qza) in QIIME 2 v2018.6 (Caporaso et al. 2010; https://qiime2.org). These files are freely available online courtesy of the QIIME Development Team and the German Network for Bioinformatics Infrastructure (https://www.arb-silva.de/download/archive/qiime).

The classifier was trained in QIIME 2 v2018.6 using scikit-learn v0.19.1 (Pedregosa et al. 2011). NOTE: The TaxonomicClassifier artifacts provided cannot be used with other versions of scikit-learn. While the classifiers may complete successfully, the results will be unreliable.

These files were created using the Mesabi HP Linux cluster at the University of Minnesota Supercomputing Institute (https://www.msi.umn.edu).

Walltimes required (hh:mm:ss) using 1 node and 24 Intel Ivy Bridge processors (~93.5 GB of RAM)
SILVA 132 = 01:25:08
SILVA 128 = 00:58:57

# These classifiers were trained using the following commands in QIIME 2-2018.6:
> qiime tools import --type 'FeatureData[Sequence]' --input-path 99_otus_16S.fasta --output-path 99_otus.qza
> qiime tools import --type 'FeatureData[Taxonomy]' --input-format HeaderlessTSVTaxonomyFormat --input-path consensus_taxonomy_7_levels_99.txt --output-path consensus_taxonomy_7_levels_99.qza
> qiime feature-classifier extract-reads --i-sequences 99_otus.qza --p-f-primer CCTACGGGAGGCAGCAG --p-r-primer ATTACCGCGGCTGCTGG --o-reads 99_otus_V3.qza
> qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads 99_otus_V3.qza --i-reference-taxonomy consensus_taxonomy_7_levels_99.qza --o-classifier 99_otus_V3_classifier.qza

# References
Citation entries formatted for LaTeX users are provided in bibliography.bib.

Bartram AK, Lynch MDJ, Stearns JC, Moreno-Hagelsieb G, Neufeld JD. Generation of multimillion-sequence 16S rRNA gene libraries from complex microbial communities by assembling paired-end Illumina reads. Appl Environ Microb 77, 3846–3852 (2011).

Caporaso JG, et al. QIIME allows analysis of high-throughput community sequencing data. Nat Methods 7, 335–336 (2010).

Muyzer G, de Waal EC, Uitterlinden AG. Profiling of complex microbial-populations by denaturing gradient gel-electrophoresis analysis of polymerase chain reaction-amplified genes coding for 16S rRNA. Appl Environ Microb 59, 695–700 (1993).

Pedregosa F, et al. Scikit-learn: Machine Learning in Python. J Mach Learn Res 12, 2825–2830 (2011).

Quast C, et al. The SILVA ribosomal RNA gene database project: improved data processing and web-based tools. Nucleic Acids Res 41, D590–D596 (2012).

Werner JJ, et al. Impact of training sets on classification of high-throughput bacterial 16S rRNA gene surveys. ISME J 6, 94–103 (2012).

Yilmaz P, et al. The SILVA and ‘All-species Living Tree Project (LTP)’ taxonomic frameworks. Nucleic Acids Res 42, D643–D648 (2013).
