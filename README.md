# bimm143-project3.r

## Welcome to my first project! 

This is a project I did for my final project in an undergrad bioinformatics course I took in the Spring of 2021. 

Essentially I was replicating the results of another study that used RNAseq data to see the effect of various cannabinoids (CBD and THC) on the immune response of microglial cells taken from the mouse brain. 
A lot of the important data processing had already been done, so I was starting with Excel sheets showing the log2 fold changes of differentially expressed genes. 
Further down the line I intend to make another project on this Git page showing the entire process, but for now we're looking at organizing the data sets and creating gene-concept networks from the data. 

### Experimental design and initial data
LPS was applied to cells to trigger an immune response. 
CBD, THC, or neither was applied along with it to modulate that immune response. 

This leaves us with 6 categories of data: 
no manipulation (control), LPS alone (control), 
CBD alone, THC alone, LPS with CBD, and LPS with THC. 

The original data looked like this: 
![xls-upgenes](https://user-images.githubusercontent.com/32500894/149410206-232516ea-8434-4cee-a8a3-a82233576e5a.png)

Functions can often be finnicky, so I had to do some reformatting within Excel and within R using regular expressions and the dplyr package. 
One particularly important step was gathering Gene IDs from the original data set and switching their original RefSeqID's for Entrez ID's using the bitr() function within clusterProfiler. 
For example: 

up.genes.entrez <- clusterProfiler::bitr(upreg_genes$RefSeqID, fromType = "REFSEQ", toType = "ENTREZID", OrgDb = org.Mm.eg.db, drop = FALSE)


### Performing Gene Ontology with clusterProfiler

Finally, with the ordered lists in hand and the gene IDs converted to Entrez format, we're ready for the GO analysis. 
For this, we're using the groupGO() function from clusterProfiler. 

Gene ontology is essentially a goal that aims to tie genes with functions, interactions, diseases and so on. 
From Wikipedia: "The Gene Ontology (GO) is a major bioinformatics initiative to unify the representation of gene and gene product attributes across all species.
More specifically, the project aims to: maintain and develop its controlled vocabulary of gene and gene product attributes; annotate genes and gene products, and assimilate and disseminate annotation data; and provide tools for easy access to all aspects of the data provided by the project, and to enable functional interpretation of experimental data using the GO, for example via enrichment analysis.

For our purposes though, GO analysis is just one of multiple steps in categorizing genes from our list. 
From a groupGO() instantiation, we're able to create simple plots showing rough categories of genes that were impacted by one experimental condition or another. 
For example:
![lps-upreg-plot](https://user-images.githubusercontent.com/32500894/149413608-a0529ddf-b21f-4a60-b977-f510ca9e535b.png)

We can also modify the groupGO() instantiation to make it readable within the R terminal, which gives access to extra information that usually stays tucked away behind the code. 

### Creating Gene Concept Networks using cnetplot()

Gene concept networks are a level above gene ontology in that we take the groupGO() instance we made earlier and apply it with the vector of fold changes to depict the links of genes and biological concepts. 
Here's some examples down below from the upregulated LPS and upregulated LPS+THC datasets. 
![upreg-lps](https://user-images.githubusercontent.com/32500894/149416022-6791649e-555e-436e-906c-f04860fe84ac.png)
![upreg-lpsthc](https://user-images.githubusercontent.com/32500894/149415913-ee6e43e5-decf-4176-957c-7e94222943a1.png)

This was more or less the end of my project. I went on to examine seven select genes related to inflammation such as IL-6, CD28 and so on.
But looking back my analysis was mediocre and malinformed, as I hadn't taken immunology yet and so I didn't have the knowledge to couldn't give specific explanations or draw interesting conclusions from the data.

### Playing around with rWikiPathways for Pathway Enrichment

One final piece that is interesting however was my quick forray into rWikiPathways, which is a package that lets me interact with an outside program called Cytoscape.
With that, we're able to download and depict some of the the involved pathways from our dataset in a process called Pathway Enrichment.
The workflow for this is relatively simple.
First we use rWikiPathways to download the pathways for Mus musculus (lab mice) along with some genes and their names. 
Then we use the enricher() function from clusterProfile to generate pathway enrichments based off our original list of ENTREZ gene ID's. 
We can generate barplots from here but one interesting application is to simply look up a gene and its pathways using rWikiPathways in browser.



First we're working with the rWikiPathways package for pathway enrichment. Step 1 is to download the pathway archive for Mus musculus, then collecting vectors of genes and names from the pathway archive. 
Then we're doing the pathway enrichment on this, but limiting to just the upregulated genes from here on out because this notebook is long enough as it is
