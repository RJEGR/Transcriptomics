---
layout: default
---

[back to Menu](../)

Based on work from [this guy](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Running-Trinity) we develope some technical adjustment in order to run trinity within the cluster at cicese. this page summarize the analysis for:


- Configure sample file
- _Denovo_ Assembly with Trinity
- Assembly quality

## [](#header-2)Configure sample file

Supose you've a set of five paired-end libraries than were processed as in [ pre-procesing step page](Processing); then, you want to assembly it with Trinity; lets write those in a tab-delimited text file indicating biological replicate relationships. You can make it using any spreadsheets tool (_excel, google etc._) or use the code below than input the list of fastq files found within your _work directory_ and use the name file per library for label the factors than expect be biological relationship indicator; then, separete those by forward (R1) and reverse (R2) ends:

```shell
ls *R2* | grep fastq | sort > R2.tmp && ls *R1* | grep fastq | sort > R1.tmp && \
cut -d "-" -f1 R1.tmp > factors.tmp && \
cut -d "_" -f1 R1.tmp > codes.tmp && \
paste factors.tmp codes.tmp R1.tmp R2.tmp | awk '{gsub(/\-/,"_",$2); \
print $1,$2,$3,$4}' | column -t > samples.file && rm *tmp
```
As results, data in table below is printed in a file with name _samples.file_; which include two factor columns at beginning and the forward and reverse name of libraries in the last two columns.


| Factor 1     | Factor 2          | Forward                 | Reverse |
|:-------------|:------------------|:------------------------|---------|
| T0           | T0_1            | T0-1_S1_L001_R1.fastq.gz  | T0-1_S1_L001_R2.fastq.gz
| T1           | T1_100_1        | T1-100-1_S2_L001_R1.fastq.gz  | T1-100-1_S2_L001_R2.fastq.gz
| T1           | T1_200_1        | T1-200-1_S3_L001_R1.fastq.gz  | T1-200-1_S3_L001_R2.fastq.gz
| T3           | T3_100_1        | T3-100-1_S4_L001_R1.fastq.gz  | T3-100-1_S4_L001_R2.fastq.gz
| T3           | T3_200_1        | T3-200-1_S5_L001_R1.fastq.gz  | T3-200-1_S5_L001_R1.fastq.gz

> The sample file is very useful when want to assembly more than 2 libraries by writing all the datasets as one file during the Trinity command.

Then, lets run the assembly including the trimmomatic step

## [](#header-2) _Denovo_ Assembly with Trinity

```shell
Trinity --seqType fq --max_memory 100G \
         --samples_file samples.file \
#         --no_normalize_reads \
         --CPU 24 \
         --output trinity_out \

```

> If you have especially large RNA-Seq data sets involving many hundreds of millions of reads to billions of reads, in silico normalization is a necessity.  as of Nov, 2016, in silico normalization in Trinity happens by default; then just turn off the `--no_normalize_reads` option if necessary or read details in the [Trinity blog](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Running-Trinity#assembling-large-rna-seq-data-sets-hundreds-of-millions-to-billions-of-reads).
>
> To details with trimmomatic read the [pre-procesing step page](Processing)

## [](#header-2) Assembly quality

 Finally, you can run the `TrinityStats.pl` from the trinity toolkit in order to determine the assembly quality and completeness by computing the Nx statistics.

```shell
TrinityStats.pl ./trinity_out/Trinity.fasta >& trinityStats.log
```

Based on the lengths of the assembled transcriptome contigs, `TrinityStats` compute the Nx length statistic, such that at least x% of the assembled transcript nucleotides are found in contigs that are at least of Nx length. The traditional method is computing N50, such that at least half of all assembled bases are in transcript contigs of at least the N50 length value.


As example, the trinityStats.log file is printed as follow:
```
################################
## Counts of transcripts, etc.
################################
Total trinity 'genes':  61694
Total trinity transcripts:      147454
Percent GC: 39.21

########################################
Stats based on ALL transcript contigs:
########################################

        Contig N10: 2818
        Contig N20: 2184
        Contig N30: 1754
        Contig N40: 1406
        Contig N50: 1121

        Median contig length: 465
        Average contig: 737.28
        Total assembled bases: 108715008


#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################

        Contig N10: 3048
        Contig N20: 2350
        Contig N30: 1892
        Contig N40: 1509
        Contig N50: 1178

        Median contig length: 392
        Average contig: 708.54
        Total assembled bases: 43712872
```
The N10 through N50 values are shown computed based on all assembled contigs. In this example, 10% of the assembled bases are found in transcript contigs at least 2,818 bases in length (N10 value), and the N50 value indicates that at least half the assembled bases are found in contigs that are at least 1121 bases in length.

The contig N50 values can often be exaggerated due to an assembly program generating too many transcript isoforms, especially for the longer transcripts. To mitigate this effect, the trinityStats script will also compute the Nx values based on using only the single longest isoform per 'gene'.


## N50

> Reference: [Elin Videvall, et. al. (2017), The molecular ecology](https://www.molecularecologist.com/2017/03/whats-n50/)

Imagine that you line up all the contigs in your assembly in the order of their sequence lengths. You have the longest contig first, then the second longest, and so on with the shortest ones in the end. Then you start adding up the lengths of all contigs from the beginning, so you take the longest contig + the second longest + the third longest and so on — all the way until you’ve reached the number that is making up 50% of your total assembly length. That length of the contig that you stopped counting at, this will be your N50 number.

We strongly advise against using regular N50 metrics for transcriptome assemblies. Instead, other more appropriate measures can be used. The developers of the transcriptome assembler Trinity have invented the ExN50 metric, which takes into account the expression levels of each contig and is therefore a more suitable contig length metric for transcriptomes.  

Although we outline above several of the reasons for why the contig N50 statistic is not a useful metric of assembly quality, below we describe the use of an alternative statistic - the ExN50 value, which we assert is more useful in assessing the quality of the transcriptome assembly. The ExN50 indicates the N50 contig statistic (as earlier) but restricted to the top most highly expressed transcripts. Compute it like so (Brian Haas, 2016):

```bash
$TRINITY_HOME/util/misc/contig_ExN50_statistic.pl Trinity_trans.TMM.EXPR.matrix \
        trinity_out_dir/Trinity.fasta > ExN50.stats
```

And plot
```bash
$TRINITY_HOME/util/misc/plot_ExN50_statistic.Rscript ExN50.stats

xpdf ExN50.stats.plot.pdf
```



## Reads represented

To assess the read composition of our assembly, we want to capture and count all reads that map to our assembled transcripts, including the properly paired and those that are not we run the process below. Bowtie2 is used to align the reads to the transcriptome and then we count the number of proper pairs and improper or orphan read alignments. First, build a bowtie2 index for the transcriptome and then perform the aligment to catpure the paired-reads aligment statistic. (Ref [here](https://github.com/trinityrnaseq/trinityrnaseq/wiki/RNA-Seq-Read-Representation-by-Trinity-Assembly))


```bash
bowtie2-build Trinity.fasta Trinity.fasta
Then perform the alignment (example for paired-end reads) to just capture the read alignment statistics.

srun bowtie2 -p 10 -q --no-unal -k 20 -x Trinity.fasta -1 R1.P.qtrim.fq -2 R2.P.qtrim.fq | samtools view -@10 -Sb -o ./bowtie2.bam

```

Finally lets summary the bam file using bamtools:

```bash
bamtools stats -in reads_represented/bowtie2.bam
```

> in case you neet, fist install bamtools as follow: https://github.com/pezmaster31/bamtools/wiki/Building-and-installing

A output in your screen will be printed as follow:

---

Stats for BAM file(s):

---

Total reads:       13970909

Mapped reads:      13970909	(100%)

Forward strand:    6885283	(49.283%)

Reverse strand:    7085626	(50.717%)

Failed QC:         0	(0%)

Duplicates:        0	(0%)

Paired-end reads:  13970909	(100%)

'Proper-pairs':    11803698	(84.4877%)

Both pairs mapped: 13127153	(93.9606%)

Read 1:            7037955

Read 2:            6932954

Singletons:        843756	(6.03938%)



> Next step is inspect the assembly quality and completeness with TransRate (Smith-Unna et al., 2016) in order to compute the E90N50 values and contigs score components.

[back to Menu](../)