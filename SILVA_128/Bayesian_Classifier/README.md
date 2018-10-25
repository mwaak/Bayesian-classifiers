# Bayesian-classifiers
**Description:** Naïve Bayesian classifier for taxonomic classification of amplicon sequences in QIIME 2.

## 16S rRNA gene V3 region using 341F/534R primers (Muyzer et al. 1993)
Taxonomic classification of 16S rRNA gene fragments may be more accurate using naïve Bayesian classifiers that have been trained on the specific fragment of interest---not the entire 16S rRNA gene (Werner et al. 2012). Reference sequences and 7-level consensus taxonomic annotation from SILVA (Quast et al. 2012; Yilmaz et al. 2013) are provided that have been trimmed to the V3 region (~170 to 200 bp) via amplification by primers 341F and 534R (Muyzer et al. 1993). 

These files were created using the 'Mesabi' HP Linux cluster at the [University of Minnesota Supercomputing Institute](https://www.msi.umn.edu).

### NOTES
The TaxonomicClassifier artifacts were trained in QIIME 2 v2018.6 using scikit-learn v0.19.1 (Pedregosa et al. 2011) and cannot be used with other versions of scikit-learn. While the classifiers may complete successfully, the results will be unreliable.

The 341F and 534R primers were first used for high-throughput, paired-end Illumina sequencing of PCR amplicons by Bartram et al. (2011), who used identical primers with different nomenclature (i.e., 341F and 518R).

## QIIME 2 commands
The SILVA 99% operational taxonomic units (`99_otus_16S.fasta`) and 7-level consensus taxonomy (`consensus_taxonomy_7_levels_99.txt`) were retrieved [here](https://www.arb-silva.de/download/archive/qiime). The 7-level taxonomic annotations use the following format: D_0__Domain;D_1__Phylum;D_2__Class;D_3__Order;D_4__Family;D_5__Genus;D_6__species

The following commands were run in QIIME 2-2018.6 (Caporaso et al. 2010; https://qiime2.org):

**(1)** Import 99% reference operational taxonomic units (OTUs) from SILVA as QIIME 2 artifact.
~~~bash
qiime tools import --type 'FeatureData[Sequence]' --input-path 99_otus_16S.fasta --output-path 99_otus.qza
~~~
**(2)** Import taxonomic annotation for the SILVA 99% OTUs as QIIME 2 artifact.
~~~bash
qiime tools import --type 'FeatureData[Taxonomy]' --input-format HeaderlessTSVTaxonomyFormat --input-path consensus_taxonomy_7_levels_99.txt --output-path consensus_taxonomy_7_levels_99.qza
~~~
**(3)** Find and remove forward and reverse primers (plus all nucleotides before/after the primers).
~~~bash
qiime feature-classifier extract-reads --i-sequences 99_otus.qza --p-f-primer CCTACGGGAGGCAGCAG --p-r-primer ATTACCGCGGCTGCTGG --o-reads 99_otus_V3.qza
~~~
**(4)** Train a naïve Bayesian classifier using the trimmed representative sequences and taxonomy.
~~~bash
qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads 99_otus_V3.qza --i-reference-taxonomy consensus_taxonomy_7_levels_99.qza --o-classifier 99_otus_V3_classifier.qza
~~~

## References
Citation entries formatted for LaTeX users are provided in `bibliography.bib`.

Bartram AK, Lynch MDJ, Stearns JC, Moreno-Hagelsieb G, Neufeld JD. Generation of multimillion-sequence 16S rRNA gene libraries from complex microbial communities by assembling paired-end Illumina reads. Appl Environ Microb 77, 3846–3852 (2011). doi: https://dx.doi.org/10.1128/AEM.02772-10

Caporaso JG, et al. QIIME allows analysis of high-throughput community sequencing data. Nat Methods 7, 335–336 (2010). doi: https://dx.doi.org/10.1038/nmeth.f.303

Muyzer G, de Waal EC, Uitterlinden AG. Profiling of complex microbial-populations by denaturing gradient gel-electrophoresis analysis of polymerase chain reaction-amplified genes coding for 16S rRNA. Appl Environ Microb 59, 695–700 (1993).

Pedregosa F, et al. Scikit-learn: Machine Learning in Python. J Mach Learn Res 12, 2825–2830 (2011).

Quast C, et al. The SILVA ribosomal RNA gene database project: improved data processing and web-based tools. Nucleic Acids Res 41, D590–D596 (2012). doi: https://dx.doi.org/10.1093/nar/gks1219

Werner JJ, et al. Impact of training sets on classification of high-throughput bacterial 16S rRNA gene surveys. ISME J 6, 94–103 (2012). doi: https://dx.doi.org/10.1038/ismej.2011.82

Yilmaz P, et al. The SILVA and ‘All-species Living Tree Project (LTP)’ taxonomic frameworks. Nucleic Acids Res 42, D643–D648 (2013). doi: https://dx.doi.org/10.1093/nar/gkt1209
