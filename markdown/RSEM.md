---
layout: default
---
[back to Menu](../)

Text can be **bold**, _italic_, or ~~strikethrough~~.

- outline 1

Both, the de novo transcriptome assembly and all pre-processed libraries were used as input to perform a sample-specific expression analysis. All reads were aligned back against the indexed de novo transcriptome assembled using Bowtie2 (Langmead and Salzberg, 2012), followed by calculation of gene and isoform expression levels using Expectation-Maximization algorithm embedded in the Trinity differential expression modules (align_and_estimate_abundance.pl) on a per sample basis.

```shell
align_and_estimate_abundance.pl --transcripts Trinity.fasta --seqType fq  \
    --est_method RSEM \
    --aln_method bowtie2 \
    --prep_reference \
    --trinity_mode \
    --samples_file samples.file \ 
    --output_dir \
    --thread_count=24 &

```

>  Provide the transcript (agreed with your Trinity.fasta assembly) reference to assess the abundance through your paired samples (it is not indexed, it will be automatically done)
> the sample file modified with the current identifier name (Ex. P.qtrim.gz prefix used within trinity --trimmomatic config

Final step is output the isoform results in a matrix than could be input in follow statement:

```
ls `path_where_rsem_results_are`/rsem/*/*isoforms.results > isoforms.results
```

Then, convert into a matrix:



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

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# [](#header-1)Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## [](#header-2)Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
