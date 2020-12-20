Installing Qiime2 from https://docs.qiime2.org/2020.11/install/download.html 

1. Import paired-end reads:
```
qiime tools import \
 --type 'SampleData[PairedEndSequencesWithQuality]' \
 --input-path manifest.tsv \
 --output-path paired-end-demux.qza \
 --input-format PairedEndFastqManifestPhred33V2
```
Output artifacts: paired-end-demux.qza

Visualize it: 
```
qiime demux summarize \
 --i-data paired-end-demux.qza \
 --o-visualization demux.qzv
```
2. Joining reads:
```
qiime vsearch join-pairs \
  --i-demultiplexed-seqs paired-end-demux.qza \
  --o-joined-sequences demux-joined.qza
```
Output artifacts: demux-joined.qza

Visualize it: 
```
qiime demux summarize \
  --i-data demux-joined.qza \
  --o-visualization demux-joined.qzv
```
3. Sequence quality control:
```
qiime quality-filter q-score \
  --i-demux demux-joined.qza \
  --o-filtered-sequences demux-joined-filtered.qza \
  --o-filter-stats demux-joined-filter-stats.qza
```
Visualize stats file: 
```
qiime metadata tabulate \
  --m-input-file demux-joined-filter-stats.qza \
  --o-visualization demux-joined-filter-stats.qzv
```
4. Trimming and denoising with Deblur:
Use demux-joined.qzv file to determinate nucleotide position in which sequences will be truncate. 
```
qiime deblur denoise-16S \
  --i-demultiplexed-seqs demux-joined-filtered.qza \
  --p-trim-length 200 \
  --p-sample-stats \
  --o-representative-sequences rep-seqs.qza \
  --o-table table.qza \
  --o-stats deblur-stats.qza
```
Output artifacts: rep-seqs.qza, table.qza, deblur-stats.qza

View summary of Deblur feature table:
```
qiime feature-table summarize \
  --i-table table.qza \
  --o-visualization table.qzv

qiime deblur visualize-stats \
  --i-deblur-stats deblur-stats.qza \
  --o-visualization deblur-stats.qzv

qiime feature-table tabulate-seqs \
  --i-data rep-seqs.qza \
  --o-visualization rep-seqs.qzv
```
5. Generete a phylogenetic tree:
```
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences rep-seqs.qza \
  --o-alignment aligned-rep-seqs.qza \
  --o-masked-alignment masked-aligned-rep-seqs.qza \
  --o-tree unrooted-tree.qza \
  --o-rooted-tree rooted-tree.qza
```
6. Taxonomic analysis:
```
qiime feature-classifier classify-sklearn \
  --i-classifier silva-138-99-515-806-nb-classifier.qza \
  --i-reads rep-seqs.qza \
  --o-classification taxonomy.qza
```
