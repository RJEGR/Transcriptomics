---
layout: default
---
[back to Menu](../)

Both, the de novo transcriptome assembly and all pre-processed libraries were used as input to perform a sample-specific expression analysis. All reads were aligned back against the indexed de novo transcriptome assembled using Bowtie2 (Langmead and Salzberg, 2012), followed by calculation of gene and isoform expression levels using Expectation-Maximization algorithm embedded in the Trinity differential expression modules (align_and_estimate_abundance.pl) on a per sample basis.

```shell
align_and_estimate_abundance.pl --transcripts Trinity.fasta --seqType fq  \
    --est_method RSEM \
    --aln_method bowtie2 \
    --prep_reference \
    --trinity_mode \
    --samples_file samples.file \ 
    --thread_count=24

```

>  Provide the transcript (agreed with your Trinity.fasta assembly) reference to assess the abundance through your paired samples (it is not indexed, it will be automatically done)
> the sample file modified with the current identifier name (Ex. P.qtrim.gz prefix used within trinity --trimmomatic config

Final step is output the isoform results in a matrix than could be input in follow statement:

```
ls `path_where_rsem_results_are`/rsem/*/*isoforms.results > isoforms.results
```

Then, convert into a matrix:

> Fist, load the source from: `SOURCE_PATH=/LUSTRE/bioinformatica_data/biocomp/gmelendrez/exports
source $SOURCE_PATH` 

```
abundance_estimates_to_matrix.pl \
        --gene_trans_map Trinity.gene_trans_map \
        --est_method RSEM \
        --out_prefix RSEM.isoforms \
        --quant_files isoforms.results.txt \
        --name_sample_by_basedir
```
> The abundance_estimates_to_matrix.pl` script may be included in the trinity/utils folder (Ex. $INSTALLATION_PATH/trinityrnaseq-Trinity-v2.5.1/util/)

> The gene_trans_map file details the relation between gene and isoforms in the assembly within two columns `<tab>` delimited file. It can be done by run get_Trinity_gene_to_trans_map.pl script in `INSTALLATION_PATH/trinity_utils/support_scripts/get_Trinity_gene_to_trans_map.pl trinity.fasta > Trinity.gene_trans_map`
