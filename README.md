# Metagenomic_analysis_BI2020

To date, a large number of methods have been developed to analyze metagenomic data. In this project compared four most popular bioinformatics pipelines: USEARCH-UPARSE(OTUs), USEARCH-UNOISE3(ASVs), Qiime2-Deblur(ASVs), and DADA2(ASVs). 

The aim of this project is a selection of the most optimal algorithm for analyzing metagenomic data (16S rRNA) on 77 PE reads(2x250) from Illumina platform.

Goals of the project:
* analyse reads with USEARCH-UPARSE pipeline;
* analyse reads with USEARCH-UNOISE3 pipeline;
* analyse reads with Qiime2-Deblur pipeline;
* analyse reads with DADA2 pipeline;
* create phyloseq objectcs from all of output files from each pipelines;
* comparative analysis of taxonomic composition, alpha- and beta-diversity:

This project was performed with:
* USEARCH  version 11.0.667 32-bit on Linux 8Gb RAM
* Qiime2-Deblur version 2020.08 on Mac OS 8Gb RAM
* DADA2 version 1.14.1 on Mac OS 8Gb RAM
* R version 3.6.3
* RStudio version 1.3.1073
* Reference database: Silva 138 99% OTUs from 515F/806R region of sequences

R-packages:
* phyloseq version 1.30.0
* qiime2R version 0.99.35
* DADA2 version 1.14.1
* ggplot2 version 3.3.2
* csv version 0.5.9
* dplyr version 1.0.2
* tidyverse version 1.3.0
* tidyr version 1.1.2
