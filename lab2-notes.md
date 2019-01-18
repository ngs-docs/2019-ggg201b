# Lab 2 notes - GGG 201(b), 2019

## Zeroth things zeroth: workshops!

On Wed Jan 30th, I will run a 3 hour "shell, R, and bioinformatics workflows" workshop, from 9-noon. I'll send out registration info early next week.

Please also reserve Wed Feb 27th for another workshop; this will probably be on snakemake.

I need to find a location to run a "shell basics" workshop, maybe on Wed Feb 20th.

## First things first - Jetstream

"Start up an m1.medium instance on Jetstream."

Long form tutorial with screenshots [here](https://angus.readthedocs.io/en/2018/jetstream/boot.html).


Briefly,

* go to [use.jetstream-cloud.org/application](https://use.jetstream-cloud.org/application) and log in.
* the username is 'diblions' and the password is in a Canvas announcement.
* create a folder with your name in it under projects, then create a new instance.
    * find the GGG image under favorites
    * change the instance name to something like lab2
    * select m1.medium under instance size
    * click 'launch'
* ...wait.

While the instance launches:

## Let's talk about workflows!

raw data -> workflow -> summary data for plotting and statistics.

* every computational workflow consists of multiple steps, taking previous outputs and/or data/information in and executing upon them and outputting something.
* usually (but not always) the data gets less as you proceed through the workflow
    * workflows are often *very* computationally intensive, and can take days or weeks to run
    * the first steps in the workflow are often the slowest...
* think about controls and qc! how do you know each step worked? evaluation is often very important.
* what reasons are there for having workflows be broken up into multiple steps rather than all in one?
* one of the things you'll learn in this class is how to use and modify automated workflows, and maybe how to build them.
* dangers of workflows: point and click and done! => black box. bad for research.

## What does a de novo RNAseq workflow look like?

* raw data: short read RNAseq
* desired output: annotated transcriptome, with abundances
* what goes between the raw data and desired output??

### major steps in short-read, de novo, RNAseq analysis:

1. trimming and error removal
2. de novo assembly
3. annotation
4. expression analysis

![](IMG_5976.jpg)
![](IMG_5977.jpg)


## Back to Jetstream

Your Jetstream instance should now be green!

(If not, I'll allocate some extra instances that I pre-started.)

Click on 'open web shell', lower right.

---

You should now be at a shell prompt! It will look something like this:

```
(base) diblions@js-168-79:~$ 
```

Hit enter a few times and it should respond!

---

### Copy and paste

For today, we're going to be using a really obnoxious copy-paste system.

In your web browser, highlight this:

```
echo hello, world
```
and copy it into your copy buffer (usually COMMAND-C).

Now, go back to the Jetstream web shell tab, and hold down shift-ctrl-option. This will open a tab on the left.

In this tab's text box, paste from your copy buffer (usually COMMAND-V or CTRL-V).

This makes it accessible to Jetstream.

Next, hold down shift-ctrl-option. The tab will go away.

And, finally, type ctrl-shift-v to paste at the command line. And hit enter.

Voila...?

(We'll show you a better way next week. But this will suffice for now.)

### Bioconda and software installation

We're going to use bioconda for software installation! See [bioconda.github.io](https://bioconda.github.io). It has all the widely used bioinformatics software packages on it. (Note: if you have a Mac, or are using an HPC, you can also use bioconda!)

I've already installed conda on these for you - it's part of the base GGG image.

Get started by configuring bioconda. Execute:

```
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

Now, let's install the trimmomatic software package.

```
conda install -y trimmomatic
```

### Download some data and unpack it

Make a working directory:
```
mkdir ~/trim
cd ~/trim
```

Download some data:
```
curl -L https://osf.io/365fg/download -o nema_subset_0Hour.zip
curl -L https://osf.io/9tf2g/download -o nema_subset_6Hour.zip
```

You can use `ls` to look at the contents of the directory.

Unpack the data:

```
unzip nema_subset_0Hour.zip
unzip nema_subset_6Hour.zip
```

And `ls` will again show the newly expanded contents.

You can use `head` to look at two of the files:

```
head *001.fastq
```

this is raw sequencing data from an Illumina sequencer!

(If we have time, we can briefly discuss the FASTQ format.)

### Trim the data

(also see [the full ANGUS 2018 lesson](https://angus.readthedocs.io/en/2018/quality-and-trimming.html))

We're going to start our data processing work by removing sequencing adapters and eliminating really low quality sequence.

First, grab the TruSeq adapter sequences:
```
cp /opt/miniconda/pkgs/trimmomatic-*/share/trimmomatic-*/adapters/TruSeq2-PE.fa .
```

And now, run Trimmomatic:

```
trimmomatic PE 0Hour_ATCACG_L002_R1_001.fastq \
        0Hour_ATCACG_L002_R2_001.fastq \
        0Hour_ATCACG_L002_R1_001.qc.fastq orphans_1 \
        0Hour_ATCACG_L002_R2_001.qc.fastq orphans_2 \
        ILLUMINACLIP:TruSeq2-PE.fa:2:40:15 \
        LEADING:2 TRAILING:2 \
        SLIDINGWINDOW:4:2 \
        MINLEN:25
```

and you should see as output,

```
...
Input Read Pairs: 2500 Both Surviving: 2499 (99.96%) Forward Only Surviving: 0 (0.00%) Reverse Only Surviving: 1 (0.04%) Dropped: 0 (0.00%)
TrimmomaticPE: Completed successfully
```

Congratulations! You've run a real bioinformatics command :).

You can read more about Trimmomatic [here](http://www.usadellab.org/cms/?page=trimmomatic), or see the lesson page from ANGUS 2018, [here](https://angus.readthedocs.io/en/2018/quality-and-trimming.html).

Questions:

* what does the output mean?
* how would you adjust this to run on the next set of files?
* how many times do you need to run trimmomatic, given the files in this directory?
* what's with the file naming scheme we're using!?
* why is the output so messy and confusing!?

## Cleaning up your Jetstream instance

My "allocation" (a computational grant) will continue to be charged as long as the machines are running.

But we're not using them for the six days!

DELETE THEM before you leave.

What does this mean? Yes, everything goes away. We'll talk more about how to deal with this in the next two weeks :)

## LAst but not least: HW 1 description.

Lab HW #1 will be due on Feb 5th, and will be to submit the end output statistics from running a full transcriptome analysis on some data.
