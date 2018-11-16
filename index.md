
# Preparation 


Activate access to required software and input data by typing in the command line:

```
source activate laccases
```
**!!! In the following sections, change '{dataset_no}' in every command to the number from your screen, e.g. for number 3 folder name will be '3_data'**

All input data files are placed in folder **~/laccases/laccases_data/INPUT_DATA/{dataset_no}_data/**. Create a new folder and copy raw data by typing:

```
mkdir {dataset_no}_data
cd {dataset_no}_data
cp ../2018_11_20/{dataset_no}_data ./
```

It contains two files (**‘ls’** command lists content of a folder) of preprocessed sequencing reads (after quality trimming and with removed sequences of adapters) named as follows:

`
dataset_{dataset_no}_R1.fastq       dataset_{dataset_no}_R2.fastq
`

Browse file content (navigate with arrows and terminate by typing **‘q’**):
```
less dataset_{dataset_no}_R1.fastq
```

# Assembly and evaluation

Assembly of short reads into longer contigs and scaffolds can be done using **Spades** software.
Spades [documentation](http://cab.spbu.ru/files/release3.13.0/manual.html) is available at its website. 

To assemble reads into contigs type the following command in the terminal:

```
spades.py -t 1 -m 5 -o spades_output --pe1-1 dataset_{dataset_no}_R1.fastq --pe1-2 dataset_{dataset_no}_R2.fastq > assembly.log 2> assembly.err
```

List the output folder to see its content:

```
ls spades_output
```

Browse ‘scaffolds.fasta’ file:

```
less spades_output/scaffolds.fasta
```


In order to analyze assembly results one can run [MetaQuast](http://bioinf.spbau.ru/metaquast) software that calculates statistics of assembly. Prepare a directory for output:

```
mkdir metaquast_output
```

and run analysis by:

```
metaquast.py --contig-thresholds 0,1000 -o metaquast_output spades_output/scaffolds.fasta > metaquast.log 2> metaquast.err
```

# Gene finding

[Prodigal](http://compbio.ornl.gov/prodigal/) is an example of gene finder and can be used to locate open reading frames in previously obtained scaffolds:

```
prodigal -p meta -f gff -a aa_orfs.fasta -d nt_orfs.fasta –i spades_output/scaffolds.fasta  -o orfs_list.gff > gene_finding.log 2> gene_finding.err
```

Browse resulting files. Documentation of **.gff** format of obtained results can be found [here](https://github.com/hyattpd/prodigal/wiki/understanding-the-prodigal-output)

# Identification of laccases

Annotation by conserved domains composition of found multiple ORFs translations may be done with batch search of Web Batch CD-search tool against [Conserved Domain Database](https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi).

Upload amino acid translations of found ORFs (name the query by dataset name and provide e-mail address for analysis reporting)

Download results (Domain hits, Full), open in OpenOffice as .tsv file and search for hits from [cupredoxin superfamily](https://www.ncbi.nlm.nih.gov/Structure/cdd/cddsrv.cgi?uid=cl19115). Identify ids of ORFs containing domains of interest and visualize full results (come back to browser, **‘Browse results’**, pick appropriate identifier and **‘Show selected queries’** in **‘full results’** view).
Verify that identified ORF contains 2-3 domains and copper binding amino acid patterns in two different domains:

| Domain  | Pattern |
| ------------- | ------------- |
| Cu_T2/3_binding | H.H.\*H.H |
| Cu_T1_T2/3_binding | H.{2,15}H.H.\*HCH.{2,15}H |

