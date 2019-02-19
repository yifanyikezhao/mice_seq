# MiSeq

* The goal is to identify mutations that occur in ovarian cancer genes in mice treated with CRISPR and to visualize the data in an informative way. The genes of interest are: ***Trp53, PTEN2, PTEN3, BRCA1.***
* There are 5 mutated mice; from each mice, 5 samples were taken from left and right ovarian tumors and 3 metastasis sites (LOT, ROT, Met#1,2,3). Each sample underwent amplicon sequencing of the 4 genes of interst. There are 100 reads in total. 

Note: The reference reads of the 4 genes, primers and sgRNA designed to perform the CRISPR are in the ***MiSeq Primers and sgRNAs.docx*** document. 

### 2019-02-19
+ processed PTEN2 samples 6-10

### 2019-02-11
#### feedback from Katie:
+ PAM off: some of the instances might just be alignment/sequencing error? For now, just mark and keep the sequence info for further investigation
+ use samples #6-10 (i.e., p53 6-10, BRCA1 6-10, PTEN2 6-10, PTEN3 6-10)
+ for now: use sample from ***PTEN2 6-10, do not merge as they are from different locations, use R1 version (different from R2 in direction of reads) because it's more accurate***

#### a few thoughts:
+ each time we run mutationFinder, we need to:
1. read (to a dataframe) and write to a ./mutations.csv - this file maps mutation name and site to mutated seq, should contain no repeat. If new mutations are identified, write to this file.
1. write to a ./output_parsed_<samplename>.csv - this file contains occurrence counts for each mutation present in this sample (for this particular gene and location)

### 2019-02-10
#### MutationFinder
+ automatic alignment
+ allows manual curation
+ generates a csv file using pandas dataframe


### 2019-02-04
#### talked with Katie and Dr. Yamanaka
+ look for mutation directly upstream of PAM sequence (e.g. first query has 2 bp del)
+ now I am thinking that I can run cas-offinder twice per file (first filter out other lab's sequence, second reduce the window size for comparison) - no it does not work due to the same-length constraint, this program only helps filtering, have to write my own code to identify the mutations precisely
+ categorize mutation types (annotate with mutation as well)
+ how much time does it take to run the whole dataset
+ learned about compound heterozygous, homozygous, heterozygous 
+ reference: figure 1 (Katie's email)

### 2019-01-17
#### cas-offinder
+ This page provides very useful info: https://github.com/snugel/cas-offinder
+ from terminal:
```
Yifans-MBP:MiSeq yifan$ ./cas-offinder
-bash: ./cas-offinder: Permission denied
Yifans-MBP:MiSeq yifan$ chmod u+x ./cas-offinder
Yifans-MBP:MiSeq yifan$ ./cas-offinder
Cas-OFFinder v2.4 (Aug 16 2016)

Copyright (c) 2013 Jeongbin Park and Sangsu Bae
Website: http://github.com/snugel/cas-offinder

Usage: cas-offinder {input_file} {C|G|A}[device_id(s)] {output_file}
(C: using CPUs, G: using GPUs, A: using accelerators)

Example input file:
/var/chromosomes/human_hg19
NNNNNNNNNNNNNNNNNNNNNRG
GGCCGACCTGTCGCTGACGCNNN 5
CGCCAGCGTCAGCGACAGGTNNN 5
ACGGCGCCAGCGTCAGCGACNNN 5
GTCGCTGACGCTGGCGCCGTNNN 5

Available device list:
Type: CPU, ID: 0, <Intel(R) Core(TM) i7-6820HQ CPU @ 2.70GHz> on <Apple>
Type: GPU, ID: 0, <Intel(R) HD Graphics 530> on <Apple>
Type: GPU, ID: 1, <AMD Radeon Pro 455 Compute Engine> on <Apple>
Yifans-MBP:MiSeq yifan$ 
```
## Next Steps:
+ parse the output txt file - done
+ extract info about mismatched bases - done
+ visualize - csv, can do better if necessary - done
+ streamline for multiple files and for all 4 genes
+ note to self: GPU is more robust in performing this task, use G not C
+ to organize the data files: use regular expression
```
$ mkdir ./R1fastq
$ mv *R1.fastq ./R1fastq/
```
## Goals:
1. compare the reads - done
1. categorize the type of mutation (insertion, deletion, substitution; also, # of bp changed) for each read - done
1. visualize the result as smth like pie chart - done
1. streamline for the entire dataset - now I think some manual curation is needed for easier downsteam analysis
