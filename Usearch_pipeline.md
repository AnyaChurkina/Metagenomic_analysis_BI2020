Download binary file of USEARCH version (11.0.667) from https://drive5.com/usearch/download.html and gunzip in in necessary folder 

Create «reads» folder for raw reads in fastq format (it should be unzip) in folder that consist USEARCH file

Merging, filtering and deduplicating (“fastx_uniques” command) are identical for UPARSE and UNOISE3 and performed once for two piplines.

Merge paired-end reads:

   ./usearch11.0.667_i86linux32 -fastq_mergepairs ./reads/*_R1*.fastq -fastq_maxdiffs 30 -relabel @  -fastqout merged.fq

Strip primers:

   ./usearch11.0.667_i86linux32 -fastx_truncate merged.fq -stripleft 19 -stripright 20 -fastqout stripped.fq

Quality filter:

   ./usearch11.0.667_i86linux32 -fastq_filter stripped.fq -fastq_maxee 1.0 -fastaout filtered.fa

Find unique reads:

   ./usearch11.0.667_i86linux32 -fastx_uniques filtered.fa -fastaout uniques.fa -relabel Uniq -sizeout

Clustering step for UPARSE:

   ./usearch11.0.667_i86linux32 -cluster_otus uniques.fa -otus otus.fa -relabel Otu

Make OTUtable for UPARSE: 

   ./usearch11.0.667_i86linux32  -otutab merged.fq -otus otus.fa -otutabout otutable.txt
 
Make OTU tree for UPARSE:

   ./usearch11.0.667_i86linux32 -calc_distmx otus.fa -tabbedout otus_dm.txt
   ./usearch11.0.667_i86linux32 -cluster_agg otus_dm.txt -treeout otus.tree

Predict taxonomy for UPARSE:

   ./usearch11.0.667_i86linux32 -sintax otus.fa -db silva_nr99_train_set.fa -strand both -tabbedout reads_taxa.txt -sintax_cutoff 0.8

Clustering step for UNOISE3:

   ./usearch11.0.667_i86linux32 -unoise3 uniques.fa -zotus zotus.fa

Make zOTUtable(denoise sequence):

   ./usearch11.0.667_i86linux32  -otutab merged.fq -zotus zotus.fa -otutabout zotutable.txt

Make OTU tree for UNOISE3:

   ./usearch11.0.667_i86linux32 -calc_distmx zotus.fa -tabbedout zotus_dm.txt
   ./usearch11.0.667_i86linux32 -cluster_agg zotus_dm.txt -treeout unoise3.tree

Predict taxonomy for UNOISE3:

   ./usearch11.0.667_i86linux32 -sintax ./out/zotus.fa -db silva_nr99_train_set.fa -strand both -tabbedout unoise3_taxa.txt -sintax_cutoff 0.8
