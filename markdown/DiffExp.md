---
layout: default
---

[back to Menu](../)

In order to identify a global patterns of differential expression transcript-level, the read counts-specific matrices obtained during abundance estimation step were used together with the R package edgeR (Robinson et al., 2010). For visualization step, ggplot and heatmap3 (Zhao et al., 2014) packages were performed in the R session.
Each count data was normalized using the trimmed mean of M-values (TMM) normalization method, which was corrected for library size and reduced RNA compositional effect with run_DE_analysis.pl, performing 0.01 as dispersion value due to the biological replicates.
Only genes represented with an adjusted P-value (FDR, False Discovery Rate) < 1e-2 and at least a two-fold change were considered as significantly differentially expressed in the pairwise comparison of the samples.


For aim this analysis lets use the version of Trinity 5.1 installed at: 

```shell
DF=/LUSTRE/apps/bioinformatica/trinityrnaseq-Trinity-v2.5.1/Analysis/DifferentialExpression/
```

Then,

```shell
module load R-3.3.1 

$DF/run_DE_analysis.pl \
      	  --matrix ${PREFIX_}.${TYPE_RESULTS_}.counts.matrix \
       	 --method edgeR \
       	 --dispersion 0.01 \
       	 --contrast $contrast_file 

```
> The contrast file include the pairwise comparisson through the conditions.
> The Dispersion value (for datasets arising from well-controlled experiments are 0.4 for human data, 0.1 for data on genetically identical model organisms or 0.01 for technical replicates

#### [](#header-4)Contrast file contrast file

|         |        |
|:--------|:-------|
| Female  | Male   |
| Female  | Undiff |
| Male    | Undiff |

Then, lets run 

```bash
cd ./edgeR*

$DF/analyze_diff_expr.pl --matrix ../RSEM.isoform.TMM.EXPR.matrix --samples ../samples.file -P 1e-3 -C 1 --order_columns_by_samples_file
```

or if error with size for map :

```bash
$DF/analyze_diff_expr.pl --matrix ../RSEM.isoform.TMM.EXPR.matrix --samples ../samples.file -P 1e-3  -C 1 --max_DE_genes_per_comparison 1000 --max_genes_clust 10000
```

Finally, count the number of diffExpr genes

```bash
wc -l diffExpr.P1e-3_C1.matrix
```

And up genes per contrast:

```bash
wc -l *UP.subset
```

#### [](#header-4) QC Samples and Biological Replicates

Once you've performed transcript quantification for each of your biological replicates, it's good to examine the data to ensure that your biological replicates are well correlated, and also to investigate relationships among your samples (Brian Haas, 2018, web:https://github.com/trinityrnaseq/trinityrnaseq/wiki/QC-Samples-and-Biological-Replicates). 

#### [](#header-4) Batch effects

Batch effects are sub-groups of measure- ments that have qualitatively different behaviour across conditions and are unrelated to the biological or scientific variables in a study. For example, batch effects may occur if a subset of experiments was run on Monday and another set on Tuesday, if two technicians were responsible for different subsets of the experiments or if two different lots of reagents, chips or instruments were used. These effects are not exclusive to high- throughput biology and genomics research, and batch effects also affect low-dimensional molecular measurements, such as northern blots and quantitative PCR. Although batch effects are difficult or impossible to detect in low-dimensional assays, high-throughput technologies provide enough data to detect and even remove them. However, if not properly dealt with, these effects can have a particularly strong and pervasive impact.

One way to quantify the affect of non- biological variables is to **examine the principal components** of the data. Principal components are estimates of the most com- mon patterns that exist across features. For example, if most genes in a microarray study are differentially expressed with respect to cancer status, the first principal compo- nent will be highly correlated with cancer status. Principal components capture both biological and technical variability and, in some cases, principal components can be estimated after the biological variables have been accounted for15. In this case, the prin- cipal components primarily quantify the effects of artefacts on the high-throughput data. Principal components can be com- pared to known variables, such as processing group or time. If the principal components do not correlate with these known variables, there may be an alternative, unmeasured source of batch effects in the data. 

**Tackling the widespread and critical impact of batch effects in high-throughput data (2010) Jeffrey T. Leek, Robert B. Scharpf, Héctor Corrada Bravo, David Simcha, Benjamin Langmead, W. Evan Johnson, Donald Geman, Keith Baggerly and Rafael A. Irizarry. NATURE REVIEWS | GENETICS**

> If you have replicates that are clear outliers, you might consider removing them from your study as potential confounders. If it's clear that you have a batch effect, you'll want to eliminate the batch effect during your downstream analysis of differential expression.

The trinity utils have the follow code to visualize some intersting patters from the abundance count matrix. Copy and paste the follow code within an script  (ex. PtR.sh ) and input both, your abundance count matrix as well as the samples.file used during your analysis. The final code syntax will be bash PtR.sh count.matrix samples.file

```bash
#!/bin/bash -ve
PROG=~/Documents/Tools/trinityrnaseq-master/Analysis/DifferentialExpression/PtR
FILE= $1 # input your count matrix
SAMPLE=$2

#if [ -e $FILE.gz ] && [ ! -e $FILE.matrix ]; then
#    gunzip -c $FILE > $FILE.matrix
#fi

# assessing read counts and genes mapped
$PROG --matrix $FILE --samples $SAMPLE --boxplot_log2_dist 1 --output boxplots

# compare replicates
$PROG --matrix $FILE --samples $SAMPLE --compare_replicates --log2  --output rep_compare

# correlation of expression across all samples
$PROG --matrix $FILE --samples $SAMPLE --log2 --sample_cor_matrix --output sample_cor

# PCA analysis
$PROG --matrix $FILE --samples $SAMPLE --log2 --prin_comp 4 --output prin_comp

# Most variably expressed genes
$PROG --matrix $FILE --samples $SAMPLE --log2 --top_variable_genes 1000 --var_gene_method anova --heatmap --center_rows --output anovaTop1k

# combine analyses
$PROG --matrix $FILE --samples $SAMPLE --log2 \
      --top_variable_genes 1000 --var_gene_method anova --output anovaTop1k --heatmap --prin_comp 3  --add_prin_comp_heatmaps 30 \
      --center_rows --output top1kvarPC3Gene30
```

> PCA is a method for compressing a lot of data into something that capture the essence of the original data. 
The goal of PCA is to draw a graph that shows how the samples are related (or not related) to each other. Please see the follow video by Josh Starmer [StatQuest: Principal Component Analysis (PCA) clearly explained (2015)](https://www.youtube.com/watch?v=_UVHneBUBW0&t=621s) to improve your knowlege in PCA.

