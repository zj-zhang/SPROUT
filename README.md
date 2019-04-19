# SPROUT

SPROUT is a machine learning algorithm to predict the repair outcome of a **CRISPR-CAS9 knockout experiment**. SPROUT accepts the DNA sequence of the *guide* as input as well as other *genomic factors*. It then predicts various statistics of the repair outcome, including: the fraction of mutant reads with an insertion/deletion, fraction of total reads with insertion/deletion, average insertion length given an insertion, average deletion length given a deletion, diversity, most likely inserted base pair and finally the edit (mutation) efficiency of the CRISPR outcome. In this repository we provide the codes neccessary to run the SPROUT algorithm as well as the set of trained models for users who want to run SPROUT in the batch mode. We have also provided an easy-to-use [SPROUT website](<https://stanford.edu/~amiralia/SPROUT/>) with graphical interfance for general users with limited access to computing resources.

Details of SPROUT is provided in the manuscript ["Systematic Characterization of Genome Editing in Primary T cells Reveals Proximal Genomic Insertions and Enables Machine Learning Prediction of CRISPR-Cas9 DNA Repair Outcomes"](<https://www.biorxiv.org/content/10.1101/404947v1.article-info>). 


This repository provides the SPROUT package which includes the scripts required to predict the DNA repair outcome as well as a complete set of trained SPROUT models. The trained models are stored in the `model` folder of the repository. The main script of the package is the `SPROUT_predict` script which loads the pretrained models, asks for input query depending on the mode selected, and outputs the predicted statistics of the repair outcome. 


# Quickstart Guide

To use the light-weight prediction tool run the `SPROUT_prediction` script and follow the instruction. The only software requirement is python 2.7.X. or later. SPROUT pipeline conveniently works in three modes depending on what input format is available to the user. Here we detail each operating mode:

**Mode (1)** The first mode takes the DNA sequence of the guide and PAM sequence (23 base pair sequence) as the input. No other input is required for this operational mode. This is ideal for nucleotide-only guide design purposes. The code asks for one of the three input operating modes.

```
Which input format do you prefer? 
(1) sgRNA sequence only
(2) sgRNA sequence + genomic features (chromatin, etc.)
(3) location on the genome and cell type 
```
For the basic mode input the digit 1.

```
Selected option:
1
```
Now the code asks for the sgRNA sequence followed by the PAM sequence.

```
Input the sgRNA sequence followed by the PAM sequence:
TATGCATGCATCGACGATCGGGG
```

The software output SPROUT's prediction results as detailed in the manuscript.

```
Here are the repair outcomes that SPROUT predicts for this guide:

Fraction of total reads with insertion  19 % 

Insertion to deletion ratio             27 %

Average insertion length                1.1 bps

Average deletion length                 12.0 bps

Diversity                               2.25 (Low)

Most likely inserted base pair          A
```

**Mode (2)** The second mode accepts the guide sequence as well as the 33 genomic features listed in the main text as the  input. The format is a string with 33 features, separated by comma. The ordering of features are as follows: 

```
GC, CpG, priPhCons, mamPhCons, verPhCons, priPhyloP, mamPhyloP, verPhyloP, GerpN, GerpS, GerpRS, bStatistic, fitCons, cHmmTssA, cHmmTssAFlnk, cHmmTxFlnk, cHmmTx, cHmmTxWk, cHmmEnhG, cHmmEnh, cHmmZnfRpts, cHmmHet, cHmmTssBiv ,cHmmBivFlnk, cHmmEnhBiv, cHmmReprPC, cHmmReprPCWk, cHmmQuies, EncExp, EncH3K27Ac, EncH3K4Me1, EncH3K4Me3, EncNucleo.
```

The details of the features are described in Supplementary Fig. 5 as adapted from the ENCODE project detailed ["http://cadd.gs.washington.edu/static/ReleaseNotes_CADD_v1.2.pdf"]. This mode of the operation of SRPOUT is ideal when both the guide sequence and the genomic factors are known, measured or a prior extracted.

```
Selected option:
2

Input the sgRNA sequence followed by the PAM sequence:
TATGCATGCATATATATATAGGG
```

Input the genomic factors separated by ',':
0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0,0,0.5,0

Here are the repair outcomes that SPROUT predicts for this guide:

Fraction of total reads with insertion 		22 %

Insertion to deletion ratio 			58 %

Average insertion length 			0.8 bps

Average deletion length 			11.5 bps

Diversity 					2.92 (Low)

Most likely inserted base pair 		T

Edit efficiency 					55 %


Mode (3) The third mode of operation only asked for the coordinate of the cut site along with the cell type. The algorithm automatically extracts the guide from human ref genome (hg38.fa). The input coordinate should be the start of the guide position (from the 3’ side of the genome). Once the guide is extracted, the softwares does a few checks to make sure about the validity of the guide and PAM sequence. Then, the software extracts the genomic factors give the cell type from a server Finally, the SPROUT algorithm is evoked given the nucleotide and the genomic factors. 



Selected option:
3

What is the target cell type?
T-cells

Which chromosome the cut site locates?
1

What location the cut site is in that chromosome?
1234567

This is the selected guide sequence:
CGTTGAGTTCGAGCTCCGATGGG

Here are the repair outcomes that SPROUT predicts for this guide:

Fraction of total reads with insertion 		17 %

Insertion to deletion ratio 			50 %

Average insertion length 			1.0 bps

Average deletion length 			8.3 bps

Diversity 					2.49 (Low)

Most likely inserted base pair 		G


