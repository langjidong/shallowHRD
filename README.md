# shallowHRD

This method uses shallow Whole Genome Sequencing (sWGS ~ 1x) and its segmentation (via ControlFREEC) to infer the Homologous Recombination status of a tumor based on the number of Large-scale State Transtions (LSTs).


## Requirements

* R installed
* Linux or MACS OS
* The following packages installed : 
  * ggpubr
  * gridExtra
  * DescTools

## Prerequisities

shallowHRD is a Rscript that can be launched from the command line. It relies on the output of [ControlFREEC](http://boevalab.inf.ethz.ch/FREEC/tutorial.html) (V. Boeava et al., 2011). 

###### Pre-processing

FASTQ files should be aligned to the hg19 (using [bwa-mem](https://github.com/lh3/bwa) algorithm). Then, The supplementary alignements and duplicates reads should be removed from the BAM files, using [Samtools](http://www.htslib.org/doc/samtools.html) and [PicardTools' MarkDuplicate](https://broadinstitute.github.io/picard/command-line-overview.html#MarkDuplicates) respectively.



## Outputs



## Publication

Submitted to BMC Bioinformatics.

"shallowHRD : a cheap and fast automatized method to evaluate Homologous Recombination Deficiency using shallow Whole Genome Sequencing" A. Eeckhoutte et al., 2019
