---
layout: default
---

De novo transcriptome assembly is the _de novo_ sequence assembly method of creating a transcriptome without the aid of a reference genome (wikipedia). The availability of Next Generation Sequencing (NGS) technologies allows researchers to capture the spatial or temporal profile of gene expresion from a huge types of biological samples. The Following markdown describe an integrative workflow analysis in RNA-seq data based on the current bioinformatic methods.

> -Ricardo Gore

## Outlines

* [Processing Raw libraries ](./markdown/Processing).
* [Running Trinity ](./markdown/denovo-Assembly).
* [Transrating Assembly ](./markdown/transrate).
* [Annotation ](./markdown/trinotate).
* [Abundance ](./markdown/RSEM).
* [Differential Expression ](./markdown/DiffExp).
* [Functional Annotation ](./markdown/DE-ontology).
* [Further analysis](https://bioconductor.org/packages/3.7/bioc/vignettes/clusterProfiler/inst/doc/clusterProfiler.html#kegg-analysis)
    -Cluster Profile
    -ReactomePA

Text can be **bold**, _italic_, or ~~strikethrough~~.

There should be whitespace between paragraphs.

### Flow-analysis diagram

![](./figures/step-step-analysis.png)


Starts working on ssh serve

```shell
ssh user@omica
```

```shell
Password: ******
```

The Center for Scientific Research and Higher Education of Ensenada (CICESE), Mexico have a computer cluster [(Paper) ](http://todos.cicese.mx/sitio/noticia.php?n=827#.WsJ-23XwZhE) with many bioinformatic apps installed within ready to implement by subscribed users.  

Please, email PhD A. Lago in order to request a cluster account (a prior authorization from your responsal)

### Trying dimplejs into my github markdown (works!!!!) : 

<head>
    <script src="http://d3js.org/d3.v4.min.js"></script>
    <script src="http://dimplejs.org/dist/dimple.v2.3.0.min.js"></script>
  </head>
  <body>
    <script type="text/javascript">
      var svg = dimple.newSvg("body", 800, 600);
      var data = [
        { "Word":"Hello", "Awesomeness":2000 },
        { "Word":"World", "Awesomeness":3000 }
      ];
      var chart = new dimple.chart(svg, data);
      chart.addCategoryAxis("x", "Word");
      chart.addMeasureAxis("y", "Awesomeness");
      chart.addSeries(null, dimple.plot.bar);
      chart.draw();
    </script>
  </body>

  <html>
<div id="chartContainer">
  <script src="/lib/d3.v4.3.0.js"></script>
  <script src="http://dimplejs.org/dist/dimple.v2.3.0.min.js"></script>
  <script type="text/javascript">
    var svg = dimple.newSvg("#chartContainer", 590, 400);
    d3.tsv("/data/example_data.tsv", function (data) {

      // Filter for a single SKU and Channel
      data = dimple.filterData(data, "SKU", "Theta 18 Pack Standard");
      data = dimple.filterData(data, "Channel", "Hypermarkets");

      // Create and Position a Chart
      var myChart = new dimple.chart(svg, data);
      myChart.setBounds(60, 30, 500, 300);
      var x = myChart.addCategoryAxis("x", "Month")
      myChart.addMeasureAxis("y", "Unit Sales");

      // Order the x axis by date
      x.addOrderRule("Date");

      // Min price will be green, middle price yellow and max red
      myChart.addColorAxis("Price", ["green", "yellow", "red"]);

      // Add a thick line with markers
      var lines = myChart.addSeries(null, dimple.plot.line);
      lines.lineWeight = 5;
      lines.lineMarkers = true;

      // Draw the chart
      myChart.draw();

    });
  </script>
</div>
</html>