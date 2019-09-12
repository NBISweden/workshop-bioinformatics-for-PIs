---
layout: default
title:  'Hands-on Exercise'
---

## Introduction
This tutorial will take you through a basic hands-on skills to be able to
- navigate in unix environment & preview folders and files
- clone Github repository
- transfer data via `scp` between Uppmax and a local computer
- transfer data via `stfp` from Grus
- run basic bioinformatics tools to assess quality of the NGS data

### Basic unix commands


### Cloning Github repository
The course website is hosted under: https://nbisweden.github.io/workshop-bioinformatics-for-PIs/.

Have you noticed "github" part in the address? In fact, in the background we have prepared and submitted all the materials for this course to a Github repository. Github, apart from hosting and tracking code, offers rendering options to a project website, like the one above.

This repository is here: https://github.com/NBISweden/workshop-bioinformatics-for-PIs. Have a look at it? Does it look familiar to the website?

To download the repository, one could use `Download Zip` button. The repository downloads as a plain folder then, one can access materials but changes cannot be tracked.

To work reproducibly with the code, keeping track of the changes, one would instead
1. clone the repository to a local computer
2. checkout a working branch to work on
3. while working modify and/or add code to the working branch
4. commit changes
5. push the branch to the Github repository
6. on Github, one would make a `Pull request` to merge changes from the working branch to the master branch.

###### To try it out
Go to Github and create an account.

```bash

# clone repository
git clone https://github.com/NBISweden/workshop-bioinformatics-for-PIs.git

# checkout out working branch, e.g. feature-olga
git checkout -b feature-olga

# navigate into the repo and to session-git folder
cd /workshop-bioinformatics-for-PIs/session-git

# create a file with your contribution e.g.
echo "Olga's contribution" > file-olga.txt

# add file
git add file-olga.txt

# commit changes
git commit -m "Initiate file-olga.txt"

# push changes
git push --set-upstream origin unix

```
Now you can go to https://github.com/NBISweden/workshop-bioinformatics-for-PIs.git and make click on a `New pull request`. Your collaborators can view the changes, ask for modifications, or incorporate the changes under the master project branch.




### Data transfer via `scp`  

### Data transfer via `stpf`

### Bioinformatics tools



#### Preparing a working directory
To get going, let's book a node, create a working directory named with your Uppmax user name `<username>` in the `/proj/g2019018/nobackup/` directory.

 NB! Remember to replace `<username>` with your Uppmax id throughout the exercise.

:computer: **Book a node.** We have reserved half a node per person.
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

In the following example *r278* is the node booked (scroll right):

{% highlight bash %}
CLUSTER: rackham
Running jobs:
   JOBID PARTITION                      NAME     USER        ACCOUNT ST          START_TIME  TIME_LEFT  NODES CPUS NODELIST(REASON)
10033342      core              interactive    agata       g2019018  R 2019-09-12T11:41:13    9:58:24      1    1 r278
{% endhighlight %}


Connect to your node:

{% highlight bash %}
ssh <node>
{% endhighlight %}

# <a name="begin"></a> RNA-seq data processing and QC tutorial
RNA-seq has become a powerful approach to study the continually changing cellular transcriptome. Here, one of the most common questions is to identify genes that are differentially expressed between two conditions, e.g. controls and treatment. In this short introductory exercise we will present a workflow for QC and processing data from an RNA-seq experiment.

* Briefly we will,

  * check the quality of the raw reads with [FastQC](#fastqc)
  * map the reads to the reference genome using [Star](#star)
  * convert between SAM and BAM files format using [Samtools](#samtools)
  * assess the post-alignment reads quality using [QualiMap](#qualimap)
  * view RNA-seq bam files in a genome browser [IGV](#igv)

##### Data description

The data you will be using in this exercise is from the paper [YAP and TAZ control peripheral myelination and the expression of laminin receptors in Schwann cells. Poitelon et al. Nature Neurosci. 2016](http://www.nature.com/neuro/journal/v19/n7/abs/nn.4316.html). In the experiments performed in this study, YAP and TAZ were knocked-down in Schwann cells to study myelination, using the sciatic nerve in mice as a model.
For the purpose of this tutorial, that is to shorten the time needed to run various bioinformatics steps, we have down-sampled the original files. We randomly sampled, without replacement, 25% reads from each sample, using fastq-sample from [fastq-tools](http://homes.cs.washington.edu/~dcjones/fastq-tools/).


<br />
[Jump to the top](#begin)

##### File preparation

First, create the new working directory and link the raw data `fastq.gz` files.

/proj/g2019018/nobackup/<username>/transcriptome/DATA/

{% highlight bash %}
cd /proj/g2019018/nobackup/<username>/
mkdir -p transcriptome/DATA

cd transcriptome/DATA

ln -s /sw/courses/ngsintro/rnaseq/main/SRR3222409_1.fastq.gz
ln -s /sw/courses/ngsintro/rnaseq/main/SRR3222409_2.fastq.gz
{% endhighlight %}


:white_check_mark: **Check** if you linked the files correctly. You now should be able to see 2 links to the .fastq.gz files.
{% highlight bash %}
ll

SRR3222409_1.fastq.gz -> /sw/courses/ngsintro/rnaseq/main/SRR3222409_1.fastq.gz
SRR3222409_2.fastq.gz -> /sw/courses/ngsintro/rnaseq/main/SRR3222409_2.fastq.gz
{% endhighlight %}
<br/>
<br />

[Jump to the top](#begin)


## <a name="fastqc"></a> FastQC: quality check of the raw sequencing reads
After receiving raw reads from a high throughput sequencing centre it is essential to check their quality. Why waste your time on data analyses of the poor quality data? Also, more importently, being aware of any quality pitfalls allows for adapting a filtering and preprocessing strategy.
FastQC provide a simple way to do some quality control check on raw sequence data. It provides a modular set of analyses which you can use to get a quick impression of whether your data has any problems of which you should be aware before doing any further analysis.

:mag: **Read** more on [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/). Can you figure out how to run it on Uppmax?  
<br />

:computer: **Create** *fastqc* folder in your _transcriptome_ directory. **Navigate to _fastqc_ folder**.
<details>
<summary>:key: Click to see suggested commands</summary>
{% highlight bash %}
cd /proj/g2019018/nobackup/<username>/transcriptome
mkdir fastqc
cd fastqc
{% endhighlight %}
</details>  
<br />

:computer: **Load** _bioinfo-tools_ and _FastQC_ modules
<details>
<summary>:key: Click to see suggested commands</summary>
{% highlight bash %}
module load bioinfo-tools
module load FastQC/0.11.5
{% endhighlight %}
</details>  
<br />

:computer: **Run** FastQC on all the .fastq.gz files located in the _transcriptome/DATA_. **Direct the output** to the  _fastqc_ folder. :bulb: Check the FastQC option for input and output files. :bulb: The bash loop comes handy again.
<details>
<summary>:key: Click to see suggested commands</summary>
{% highlight bash %}
for i in /proj/g2018002/nobackup/<username>/transcriptome/DATA/*
do
fastqc $i -o /proj/g2018002/nobackup/<username>/transcriptome/fastqc/
done
{% endhighlight %}
</details>  
<br />

:mag: **Download** the FastQC for the proceeded sample from Uppmax to your compute and **have a look** at it. **Go back** to the [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) website and **compare** your report with [Example Report for the Good Illumina Data](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and [Example Report for the Bad Illumina Data](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html) data.  
<br />

:open_mouth: Discuss whether you'd be happy when receiving this very data from the sequencing facility.
<br />
<br />
[Jump to the top](#begin)




{% highlight bash %}

{% endhighlight %}