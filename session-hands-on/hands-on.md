---
layout: default
title:  'Hands-on Exercise'
---

## Introduction
This tutorial will take you through a basic hands-on skills to be able to:

1. navigate in unix environment & preview folders and files
2. transfer data via `scp` between Uppmax and a local computer
3. run basic bioinformatics tools to assess quality of the NGS data
4. clone Github repository

## Basic unix commands
We will practice on Rackham, Uppmax.

**Logging in**
```bash
ssh -Y username@rackham.uppmax.uu.se
```

Now your screen should look something like this:
```
_   _ ____  ____  __  __    _    __  __
| | | |  _ \|  _ \|  \/  |  / \   \ \/ /   | System:    rackham1
| | | | |_) | |_) | |\/| | / _ \   \  /    | User:      olga
| |_| |  __/|  __/| |  | |/ ___ \  /  \    |
\___/|_|   |_|   |_|  |_/_/   \_\/_/\_\   |

###############################################################################

       User Guides: http://www.uppmax.uu.se/support/user-guides
       FAQ: http://www.uppmax.uu.se/support/faq

       Write to support@uppmax.uu.se, if you have questions or comments.
```

**Moving and looking around**

When you connect to UPPMAX, you will start out in your home folder. The absolute path to your home folder is usually **/home/\<username\>**

`pwd` command, to see where you are use `pwd` command, abbreviation for **p**rint **w**orking **d**irectory
```bash
pwd
```
This is what I see
```bash
[olga@rackham1 ~]$ pwd
/home/olga
```


`ls` command, to list directory content, abbreviation for **l**i**s**t.

```bash
ls

# ls with some options, indicated by a hyphen
ls -l
ls -l -h
ls -lh
```

You should have something similar to:
```bash
[olga@rackham1 ~]$ ls -l
total 44
drwxr-xr-x 2 olga olga    4096 Aug  7 11:01 bin
drwxrwxr-x 3 olga olga    4096 Sep 10 09:12 example-data
drwxr-x--- 3 olga olga    4096 Mar  2  2017 glob
drwxr-xr-x 8 olga olga    4096 Sep  6 10:04 homer-ctcf-04
drwxrwxr-x 4 olga olga    4096 Jan 28  2014 local
drwxrwxr-x 2 olga courses 4096 Nov 21  2017 olga
drwxrwxr-x 3 olga olga    4096 Feb 13  2013 opt
drwx--S--- 2 olga olga    4096 Jan 18  2017 private
drwxrwxr-x 6 olga olga    4096 Oct 15  2018 R
drwxrwxr-x 7 olga olga    4096 Aug  9 16:32 tools
drwxrwxr-x 4 olga olga    4096 Jan 27  2019 transfer-mast
```

`man` command. In the above examples, as you may have noticed, -l or -h is an option of the command `ls` that alters its default behaviour. To read about a command and available options we use another command `man`, abbreviation for **man**ual
```bash
# manual for ls
man ls

# manual for pwd
man pwd

# manual for man
man man

```


More basic commands:
```bash
# Let's create a folder for this tutorial (mkdir, from make directories)
mkdir bip

# Let's navigate to this folder (cd, from change directory)
cd bip

# Print pathway to the folder
pwd

# Go back to the home directory (cd with no options)
cd

# Go back to bip using pull pathway
cd /home/username/bip

# Go back to home directory using pull pathway
cd /home/username/
```

What do you have in the folder? How do you check again?

**Running a script**

We have prepared some files to practice with and a small script that links these files to your home directory.

```bash
# go to home directory and "bip" folder
cd ~/bip

# copy setup.sh script
cp /sw/share/compstore/courses/bip/hands-on/scripts/setup.sh .

# run script
./setup.sh

# check content of the "bip" folder
ls -lh

# check content of the "bip/data"
ls -lsh data
```

**Looking into files**

`head`, `tail` and `less` command come handy when looking into files

```bash

# navigate to data directory
cd ~/bip/data

# output the first 10 lines of the .fastq file
head -n 10 P12516_101_R1.sample.fastq

# output the last 20 lines fo the .fastq file
tail -n 20 P12516_101_R1.sample.fastq

# preview .bed files
less CGATGG.bed

# while previewing the CGATG.bed file type search for entries with chr11
./chr11

# have a look at microRNA annotations file
less hsa.gff3

# can you find "hsa-miR-608"?
./hsa-miR-608

# what happens when you use grep command?
grep hsa-miR-608 hsa.gff3
```

**Data size**
```bash

# Navigate to "bip" folder
cd ~/bip

# estimate file space usage
du DATA/

# try some useful du options
du -a
du -h
du -ah

```

-  * To read more: [https://scilifelab.github.io/courses/ngsintro/1905/slides/linux-tutorial.pdf](https://scilifelab.github.io/courses/ngsintro/1905/slides/linux-tutorial.pdf)
- * Useful to have: [https://scilifelab.github.io/courses/ngsintro/common/files/Bash_cheat_sheet_level1.pdf](https://scilifelab.github.io/courses/ngsintro/common/files/Bash_cheat_sheet_level1.pdf)
- * To practice more: [https://scilifelab.github.io/courses/ngsintro/1905/labs/linux-intro](https://scilifelab.github.io/courses/ngsintro/1905/labs/linux-intro)

## Data transfer
`scp`, secure copy is used to copy files between hosts on a network

To try it out, open in parallel a terminal window on your local computer and Uppmax terminal

```bash
# to transfer a file from Rackham to local current directory (denoted by a dot)
scp <userame>@rackham.uppmax.uu.se:/home/<username>/bip/data/example.bed  .

# to transfer files from Rackham to local current directory (denoted by a dot)
scp -r <userame>@rackham.uppmax.uu.se:/home/<username>/bip/data .

# to transfer files in data directory from local to Rackham home directory
scp -r data/ <username>@rackham.uppmax.uu.se:/<username>/olga

```

## Cloning Github repository
The course website is hosted under: [https://nbisweden.github.io/workshop-bioinformatics-for-PIs/](https://nbisweden.github.io/workshop-bioinformatics-for-PIs/). Have you noticed "github" part in the address? In fact, in the background we have prepared and submitted all the materials for this course to a Github repository. Github, apart from hosting and tracking code, offers rendering options to a project website, like the one above. The course Github repository is here: [https://github.com/NBISweden/workshop-bioinformatics-for-PIs](https://github.com/NBISweden/workshop-bioinformatics-for-PIs). Have a look at it? Does it look familiar to the website?

To download the repository, one could use `Download Zip` button. The repository downloads as a plain folder then, one can access materials but changes cannot be tracked. To work reproducibly and collaboratively with the code, keeping track of the changes, one would instead
1. clone the repository to a local computer
2. checkout a working branch to work on
3. while working modify and/or add code to the working branch
4. commit changes
5. push the branch to the Github repository
6. on Github, one would make a `Pull request` to merge changes from the working branch to the master branch.

##### To try it out
Go to [https://github.com](https://github.com) and create an account.

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
Now you can go to https://github.com/NBISweden/workshop-bioinformatics-for-PIs.git and  click on a `New pull request`. Your collaborators can view the changes, ask for modifications, or incorporate the changes under the master project branch.

P.S. The above will also work on a local computer with .git installed

- * Read more: [https://coderefinery.github.io/git-intro/](https://coderefinery.github.io/git-intro/)



## <a name="begin"></a> Bioinformatics tools: RNA-seq data processing and QC tutorial

##### Preparing a working directory
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


##### <a name="fastqc"></a> FastQC: quality check of the raw sequencing reads
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
