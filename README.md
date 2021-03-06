# shallowHRD

This method uses shallow Whole Genome Sequencing (sWGS ~ 0.4-1x) and the segmentation of a tumor genomic profile to infer the Homologous Recombination status of a breast, ovarian tumor based on the number of Large-scale Genomic Alterations (LGAs), evaluated in a similar way to LSTs (Large-scale State Transitions) but independent of the ploidy, with no reference to an absolute copy number. This can also be applied to pancreatic and prostate tumor.

## Introduction

*shallowHRD* is a R script that can be launched from the command line. It relies on a ratio file characterizing the normalized read counts of a shallow Whole Genome Sequencing (0.4-1x) in sliding windows along the genome and its segmentation. It was developped on the output of [ControlFREEC](http://boevalab.inf.ethz.ch/FREEC/tutorial.html) ([Boeva,V. et al., 2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3268243/)) but is adapted to similar softwares (see sections "run shallowHRD" and "Nota Bene"). 

Softwares such as ControlFREEC count reads in sliding windows, normalize read count and then segment the genomic profile. *shallowHRD*, based on a inferred cut-off representing a one copy difference, will smooth the segmentation in a step wise manner, using first large segments, reintegrating small segments afterwards and then filtering small interstitial CNAs. The HR status is estimated based on the number of Large-scale Genomic Alterations (LGAs) i.e. intra-chromosome arm CNA breaks along the genome.

## Requirements

* R installed (tested with v.3.5)
* The following packages installed : 
  * ggpubr (tested with v.0.2.3)
  * gridExtra (tested with v.2.3)
  * DescTools (tested with v.0.99.28)

Tested on Linux, Mac and Windows.

## Prerequisities

First, FASTQ files should be aligned to a reference genome (hg19 or hg38) (using [BWA-MEM](https://github.com/lh3/bwa) for instance) and supplementary & duplicate reads removed from the BAM files, using [Samtools](http://www.htslib.org/doc/samtools.html) and [PicardTools' MarkDuplicates](https://broadinstitute.github.io/picard/command-line-overview.html#MarkDuplicates), respectively.

Then, the BAM file should then be processed by a software such as ControlFREEC. The recommended options for controlFREEC are indicated in a config file example in the repository (*controlfreec_config_file_example_hg19.txt*). The window size was fixed here to 20kb (coverage > 0.4x) and the parameters were set for a sensitive segmentation. The window size can however be increased up to ~60kb if necessary depending on the coverage, with a step size half its length.

Finally, the file *cytoBand_adapted_hg19.csv* or *cytoBand_adapted_hg38.csv* (available in the repository) has to be downloaded. 

The R packages needed can be installed with the script *install_packages.R* (in repository) and the command line :

```
/path/to/Rscript /path/to/install.packages.R
```

## Run *shallowHRD*

To run *shallowHRD* only one ratio file is needed (formated in ControlFREEC's output).

The name of the file should be in this format : *SAMPLE_NAME.bam_ratio.txt*. <br/>

*shallowHRD* will rely on the *first four columns* of the input file (tabulated and with column Chromosome *in number*) : <br/>
Chromosome &nbsp; Start &nbsp; Ratio &nbsp; RatioMedian <br/>
1 &nbsp;&nbsp; 1 &nbsp;&nbsp;&nbsp; -1 &nbsp;&nbsp;&nbsp; -1 <br/>
1 &nbsp;&nbsp; 20001 &nbsp;&nbsp; -1 &nbsp;&nbsp; -1 <br/>
. &nbsp;&nbsp; . &nbsp;&nbsp; . &nbsp;&nbsp; . <br/>
. &nbsp;&nbsp; . &nbsp;&nbsp; . &nbsp;&nbsp; . <br/>

The command line to launch *shallowHRD* is (absolute or relative paths) :

```
/path/to/Rscript /path/to/shallowHRD_hg19.R /path/to/SAMPLE_NAME.bam_ratio.txt /path/to/output_directory /path/to/cytoBand_adapted_hg19.csv
```
For Windows, it will be with /path/to/Rscript.exe.

Two examples in hg19 and one example in hg38 are downloadable in the repository to try *shallowHRD*.

## Outputs

All the figures and files created by the script will be available in the output directory.

The summary plot figure recapitulating all the information will look like this :

![alt text](https://github.com/aeeckhou/shallowHRD/blob/master/example1_summary_plot_hg19.jpeg)

A : Genomic profile with LGAs in green (the entire processed segmentation is represented in red if there are no LGA) <br/>
B : Density representing pairwise comparison between large segments used to fix the difference for a copy level <br/>
C : Graphe representing the value of each final segment (small blue circles) - <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the segmentation is good, the different copy number should appear clearly with disctinct steps <br/>
D : Table recapitulating different data, including the case quality and the final diagnostic for the HR status

## Nota Bene

1. Two different script of *shallowHRD* are available depending on the reference genome used

2. *shallowHRD* can be adapted to other software with slight modification of outputs to match *shallowHRD* intput format <br/> 
For instance [QDNAseq](https://github.com/ccagc/QDNAseq) (Adapt output and comment the two lines of script after "No log2 transformation") <br/>
 (Sub-section "Fast-gathering" of shallowHRD.R - 12/12/2019) <br/>

3. The overall pipeline works also on WGS with a higher coverage

## Contact

Don't hesitate to contact us for any questions, problems or adaptation of the method !

alexandre.eeckhoutte@curie.fr <br/>
tatiana.popova@curie.fr <br/>
marc-henri.stern@curie.fr


## Publication
Accepted in [Bioinformatics](https://academic.oup.com/bioinformatics) as an Application Note.

Alexandre Eeckhoutte, Alexandre Houy, Elodie Manié, Manon Reverdy, Ivan Bièche, Elisabetta Marangoni, Oumou Goundiam, Anne Vincent-Salomon, Dominique Stoppa-Lyonnet, François-Clément Bidard, Marc-Henri Stern, Tatiana Popova, ShallowHRD: Detection of Homologous Recombination Deficiency from shallow Whole Genome Sequencing, Bioinformatics,btaa261, https://doi.org/10.1093/bioinformatics/btaa261 
