---
layout: default
title:  'Hands-on Exercise'
---


# <a name="begin"></a> RNA-seq data processing tutorial
RNA-seq has become a powerful approach to study the continually changing cellular transcriptome. Here, one of the most common questions is to identify genes that are differentially expressed between two conditions, e.g. controls and treatment. In this short introductory exercise we will present a workflow for QC and processing data from an RNA-seq experiment.
Briefly we will,

  * check the quality of the raw reads with [FastQC](#fastqc)
  * map the reads to the reference genome using [Star](#star)
  * convert between SAM and BAM files format using [Samtools](#samtools)
  * assess the post-alignment reads quality using [QualiMap](#qualimap)
 

# Signal Visualisation in Genome Browser (IGV)


