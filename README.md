# GSEA
Gene Set Enrichment Analysis

Performs GeneSet Enrichment Analysis (GSEA) based on one-tail Fisher's Exact Test. Written in R.

Read vignette.Rmd file for detailed description and instructions in using the R-function


##get.enrichment.test

####Decription:

Performs GeneSet Enrichment Analysis (GSEA) based on one-tail Fisher's Exact Test

####Usage
 
``` 
get.enrichment.test(file.query, batch, file.background, threshold, dir.gmt, dir.out)
```

####Arguments

- `file.query` : path of the file containing list of genes for which enrichment test is to be performed. One Gene in a line.
- `batch` :name of output folder. This folder will be under the folder `dir.out`
- `file.background` : path of file containing list of background genes. One Gene in a line.
- `threshold` : threshold of FDR pvalue cutoff 
- `dir.gmt` : path of folder containing the genesets GMT files
- `dir.out` : path of output folder. Defaults to ''enrichment'' folder.


####Value 

The output file is a tab separated table containing following columns:

- `Category` :  Name of the Geneset
- `pvalue` : pvalue of fisher's exact test
- `fdr` : false discovery rate adjusted pvalue, uses ''Benjamini Hochberg'' method for pvalue correction.
- `overlap.percent` : proportion of query genes that are also found in the enriched geneset (expressed in percentage).
- `overlap.genes` : list of query genes that are also found in the enriched geneset
- `Description` : Description of the enriched geneset


##get.fisher.exact.test

####Decription:

Performs one-tail Fisher's Exact Test

####Usage
 
``` 
get.fisher.exact.test(dat.genesets, genes.queryset, genes.refset, ct)
```


####Arguments

- `dat.genesets` : data frame containing genesets
- `genes.queryset` : list of genes for which enrichment test is to be performed
- `genes.refset` : list of background set of genes
- `ct` : threshold of FDR pvalue cutoff  


####Value 

The output file is a tab separated table containing following columns:

- `Category` :  Name of the Geneset.
- `pvalue` : pvalue of fisher's exact test.
- `fdr` : false discovery rate adjusted pvalue, uses ''Benjamini Hochberg'' method for pvalue correction.
- `overlap.percent` : proportion of query genes that are also found in the enriched geneset (expressed in percentage).
- `overlap.genes` : list of query genes that are also found in the enriched geneset.
- `Description` : Description of the geneset.




##parseGMT

####Decription:

Parse GMT file in appropriate format for enrichment test

####Usage
 
``` 
parseGMT(gmt.name, dir.gmt)
```

####Arguments

- `gmt.name` : name of the folder containing GMT files. This folder should be under `dir.gmt`.
- `dir.gmt` : path of folder under which genesets are stored. Defaults to ''genesets'' folder.


####Value 

The output file is a tab separated table containing following columns:

- `Category` :  Name of the Geneset
- `Genesets` : list of genes in the geneset.
- `Description` : Description of the geneset.


##Example

Load the R script `run_GSEAfisher.R`

```{r echo=TRUE, message=FALSE, warning=FALSE}
source("./run_GSEAfisher.R")
```

Parse GMT files. To be used only once for a file. If the GMT files are already parsed, skip this step.

```{r}
parseGMT(gmt.name="Msigdb")
```

Now run the enrichment test
```{r}
# Defile Paths
dir.gmt <- "./genesets/Msigdb"
file.background <- "./backgroundset/background_genelist_test.txt"
file.query <- "./data/genelist_test.txt"
batch <- "TEST"

# Perform Enrichment Test
get.enrichment.test(file.query, batch, file.background, threshold=0.001, dir.gmt)
```

The output files can be found under `./enrichment/TEST` folder.


