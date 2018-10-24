---
layout: default
---

[back to Menu](../)

 (Text In process, the code works well!)


```shell
SOURCE=/LUSTRE/bioinformatica_data/genomica_funcional/scripts/exports
source $SOURCE

get_Trinity_gene_to_trans_map .pl Trinity.fasta > Trinity.fasta.gene_trans_map

Build_Trinotate_Boilerplate_SQLite_db.pl Trinotate && 

$BLAST/makeblastdb -in uniprot_sprot.pep -dbtype prot 

gunzip Pfam-A.hmm.gz && $HMM/hmmpress Pfam-A.hmm

#now lets to run full trinity annotation as follow

autoTrinotate.pl --Trinotate_sqlite Trinotate.sqlite \
                        --transcripts Trinity.fasta \
                        --gene_to_trans_map Trinity.gene_trans_map \
                        --conf conf.txt \
                        --CPU 120

```

[Link to another page](another-page).

