---
layout: default
---

[back to Menu](../)

In order to identify a global patterns of differential expression transcript-level, the read counts-specific matrices obtained during abundance estimation step were used together with the R package edgeR (Robinson et al., 2010). For visualization step, ggplot and heatmap3 (Zhao et al., 2014) packages were performed in the R session.
Each count data was normalized using the trimmed mean of M-values (TMM) normalization method, which was corrected for library size and reduced RNA compositional effect with run_DE_analysis.pl, performing 0.01 as dispersion value due to the biological replicates.
Only genes represented with an adjusted P-value (FDR, False Discovery Rate) < 1e-2 and at least a two-fold change were considered as significantly differentially expressed in the pairwise comparison of the samples.



```shell
module load R-3.3.1 

run_DE_analysis.pl \
      	  --matrix ${PREFIX_}.${TYPE_RESULTS_}.counts.matrix \
       	 --method edgeR \
       	 --dispersion 0.01 \
       	 --contrast $contrast_file 

```
> The contrast file include the pairwise comparisson through the conditions;

#### [](#header-4)Contrast file contrast file

|         |        |
|:--------|:-------|
| Female  | Male   |
| Female  | Undiff |
| Male    | Undiff |



> DF=/LUSTRE/apps/bioinformatica/trinityrnaseq-Trinity-v2.5.1/Analysis/DifferentialExpression/


then, lets run 

```
cd ./edgeR*

$DF/analyze_diff_expr.pl --matrix ../RSEM.isoform.TMM.EXPR.matrix --samples ../samples.file -P 1e-3 -C 1 --order_columns_by_samples_file
```

or if error with size for map :

```
$DF/analyze_diff_expr.pl --matrix ../RSEM.isoform.TMM.EXPR.matrix --samples ../samples.file -P 1e-3  -C 1 --max_DE_genes_per_comparison 1000 --max_genes_clust 10000
```

Finally, count the number of diffExpr genes

```
wc -l diffExpr.P1e-3_C1.matrix
```

And up genes per contrast:

```
wc -l *UP.subset
```

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](another-page).

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
