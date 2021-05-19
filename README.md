# expressyouRcell
expressyouRcell is a unique and easy-to-use R package which provides an intuitive approach for visualizing and presenting multi-dimensional variations of gene expression levels across time and space. This tool gives the possibility of generating animations of pictographic representations of cells, or pictograms, providing a convenient and intuitive method for visualizing and understanding time course variations in cellular compartments. 
A range of customizable options is provided to create cellular pictograms starting from your data! :)

## 1) Load the package
To load expressyouRcell run:
```
library(expressyouRcell)
```

## 2) Create the gene-localization table
This step can be skipped if you provide your own table with information on the localization of genes within the cellular compartments and organelles. If you provide your own table, this must contain two columns: one with gene names (named exactly "gene_symbol") and one with the associated information of the localization of that gene within the cell (named exactly subcell_struct).

Otherwise, you can create the gene-localization table with the map_gene_localization function provided within expressyouRcell. As far as now, the annotation is working only for mouse.

### map_gene_localization function
A gene annotation file, in GTF format, is required as input. On this complete set of gene symbols, a gene ontology enrichment analysis is performed to associate a gene with a term in the cellular component ontology. For this purpose, only the sub-ontology of the cellular components is taken into consideration. This step generates the gene-localization table, which maps each gene to the locations in the cellular structures, either cellular compartments or macromolecular complexes. 

## 3) Prepare your data
expressyouRcell is optimized for representing multiple sets of gene expression data (e.g. multiple stages). For this reason, the input has to be organized as a list of data.tables. For example, in case of multiple stages, each data.table should correspond to a specific time point. 
Each data.table must have at least a column of gene names named precisely “gene_symbol”.

### plot_cell function
This function simply allows the user to visualize the chosen cellular structure with the default colors. The function requires as input the data.table with the graphical information (coordinates and colors for the cellular organelles). 

Example of usage:
```
plot_cell(coords_dt = neuron_dt_nocyto)
```

## color_cell
The main function is called color_cell and needs three mandatory input parameters.
* A list of data.tables, each corresponding to a time point and must have at least a column named “external_gene_name” with gene symbols.
* A data.table containing x and y coordinates, name of the subcellular structures and an associated default color.
* A data.table storing for each gene the mapping to a subcellular localization according to the cellular component gene ontology. 
Different methods for assigning colors to subcellular localizations are then provided. 

### Mean (or median) of values
Genes are grouped according to their localization and mean (or median) of numeric values associated with each gene is computed for each group. If one of these methods are chosen, ```data.table``` in the input list must also have additional columns containing numeric values (e.g. logFC , CPM values, etc.). An additional parameter with the name of the column (named ```col_name```)on which the user wants to base the color of cellular localizations must be provided as input. The given name must be compatible with column names in the datatables of the input list. 
If genes are associated with categorical classes (e.g. up/down-regulation for DEGs classification), separated analysis for each subset of genes can be obtained by specifying as ```group_by``` parameter the name of the column with the categorical variable (e.g. “class”). 
Default value of this parameter is null. In this case, no grouping by category is performed and values of genes mapped to each subcellular localization are averaged regardless their classification. 

An additional parameter ```grouping_vars``` can be specified to select the categories the user is interested to plot (e.g. in case of DEGs classification, “up” and “down”). 
Default value of this parameter is null. In this case, genes belonging to the specified categories are subselected and their corresponding values are averaged for each subcellular localization.

### Enrichment based p-value
Enrichment analysis restricted to the sub-ontology of cellular components is performed on genes input by the user. Colors of each subcellular compartment are based on pvalues from the Fisher’s test, used to assess the statistical significance of the enrichment.


## create_animation
Create animation between stages
