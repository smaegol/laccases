---
title: Laccases 
layout: post
---


# Preparation 

All input data files are in folder {dataset_no}_data/. Copy content by typing:

```
cp INPUT_DATA/{dataset_no}_data ./
```

It contains two files of preprocessed sequencing reads (after quality trimming and with removed sequences of adapters) named as follows:

```
dataset_{dataset_no}_R1.fastq
dataset_{dataset_no}_R1.fastq
```

# Assembly

Assembly of short reads into longer contigs and scaffolds can be done using **Spades** software with a following command:

```
spades.py -t 1 -m 5 -o spades_output --pe1-1 dataset_{dataset_no}_R1.fastq 
--pe1-2 dataset_{dataset_no}_R2.fastq > assembly.log 2> assembly.err
```


In order to analyze assembly results one can run MetaQuast software that calculates statistics of assembly. Prepare a directory for output and run analysis by:

```
mkdir metaquast_output
metaquast.py --contig-thresholds 0,1000 -o metaquast_output spades_output/scaffolds.fasta > metaquast.log 2> metaquast.err
```

# Gene finding

**Prodigal** is an example of gene finder and can be used to locate open reading frames in previously obtained scaffolds:

```
prodigal -p meta -f gff -a aa_orfs.fasta -d nt_orfs.fasta –i spades_output /scaffolds.fasta 
-o orfs_list.gff > gene_finding.log 2> gene_finding.err
```

# Annotation

Annotation by conserved domains composition of found multiple ORFs translations may be done with batch search of Web CD-search tool against Conserved Domain Database on website: [https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi].

pload amino acid translations of found ORFs (please name the query by dataset name and provide e-mail address for analysis reporting)
Download results (Domain hits, Full), open in Excell/Open office as .tsv file and search for hits from cupredoxin superfamily ([https://www.ncbi.nlm.nih.gov/Structure/cdd/cddsrv.cgi?uid=cl19115]). Identify ids of ORFs containing domains of interest and visualize full results (come back to browser, ‘Browse results’, pick appropriate identifier and ‘Show selected queries’ in ‘full results’ view).
Verify that identified ORF contains 2-3 domains and copper binding amino acid patterns in two different domains:

| Domain  | Pattern |
| ------------- | ------------- |
| Cu_T2/3_binding | H.H.*H.H |
| Cu_T1_T2/3_binding | H.{2,15}H.H.*HCH.{2,15}H |

