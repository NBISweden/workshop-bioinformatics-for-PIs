---
layout: default
title:  'Hands-on Exercise'
---


# <a name="begin"></a> RNA-seq data processing and QC tutorial
RNA-seq has become a powerful approach to study the continually changing cellular transcriptome. Here, one of the most common questions is to identify genes that are differentially expressed between two conditions, e.g. controls and treatment. In this short introductory exercise we will present a workflow for QC and processing data from an RNA-seq experiment.

* Briefly we will,

  * check the quality of the raw reads with [FastQC](#fastqc)
  * map the reads to the reference genome using [Star](#star)
  * convert between SAM and BAM files format using [Samtools](#samtools)
  * assess the post-alignment reads quality using [QualiMap](#qualimap)
  * view RNA-seq bam files in a genome browser [IGV](#igv)

# Data description

The data you will be using in this exercise is from the paper [YAP and TAZ control peripheral myelination and the expression of laminin receptors in Schwann cells. Poitelon et al. Nature Neurosci. 2016](http://www.nature.com/neuro/journal/v19/n7/abs/nn.4316.html). In the experiments performed in this study, YAP and TAZ were knocked-down in Schwann cells to study myelination, using the sciatic nerve in mice as a model.
For the purpose of this tutorial, that is to shorten the time needed to run various bioinformatics steps, we have down-sampled the original files. We randomly sampled, without replacement, 25% reads from each sample, using fastq-sample from the [fastq-tools](http://homes.cs.washington.edu/~dcjones/fastq-tools/) tools.


<br />
[Jump to the top](#begin)

## Preparing a working directory
To get going, let's book a node, create a working directory named _transcriptome_ in the _/proj/g2018002/nobackup/<username>/_ directory and link the raw sequencing files .fastq.gz. NB! Remember to replace <username> with your uppmax id throughout the exercise.

:computer: **Book a node.** As for other tutorials in this course we have reserved half a node per person. 
Please book a node only once, as otherwise you'll be taking resources from your fellow course participants.

<details>
<summary>:key: Click to see how to book a node</summary>
{% highlight bash %}
salloc -A g2019018 -t 03:30:00 -p core -n 10 --no-shell --reservation=g2019018_1
{% endhighlight %} 
</details>  
<br />

Now you can check which node you have booked:

{% highlight bash %}
jobinfo -u <username>
{% endhighlight %} 

In the following example ** is the node booked:

```
Waiting jobs:
   JOBID    POS PARTITION                      NAME     USER        ACCOUNT ST          START_TIME   TIME_LEFT PRIORITY CPUS NODELIST(REASON)     FEATURES DEPENDENCY
10033342    548      core              _interactive    agata       g2019018 PD 2019-09-12T17:11:14    10:00:00   152631    1      (Resources)       (null) 



Connect to your node:

{% highlight bash %}
ssh <node>
{% endhighlight %} 
