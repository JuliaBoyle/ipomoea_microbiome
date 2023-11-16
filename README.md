# ipomoea_microbiome
This repository includes code and data for analysing Ipomoea hederacea's leaf microbiome and identifying heritable microbes.

The file "Leaf shape microbiome.rmd" contains all code neccessary to reproduce the analysis, commented throughout.
The file "ipomoea-taxa-metadata-allnumbers.csv" contains the full metadata of the experiment, including all plants that survived and died during the experiment.
The file "ipomoea_metadata.tsv" contains the metadata of only plants that survived, and are included in the analysis.
The file "table-try3-10readsmin-filtered-cyano-mito-glory.qza" is an output from QIIME2 and contains the feature table/ASV table. 
The file "rooted-tree-try3-glory.qza" is an output from QIIME2 and contains the rooted tree of taxa in the sequencing data.
The file "taxonomy-try3-glory.qza" is an output from QIIME2 and contains the assigned Greengenes taxonomy linking ASV to classification.
The file "taxonomy_try3.csv" is a csv file version of the taxonomy classification.
The file "table-try3-10readsmin-filtered-cyano-mito.csv" is a csv file version of the feature table.
The file "phygloryCLRGenfeaturetable.csv" was created in R. It contains the abundances of genera found in at least 30% of samples, and this is the manually-transposed version with samples as rows. This file is read back into R, then centered log ratio transformed for downstream analysis.


# In the metadata, this is the meaning of columns:
-First column: Unique plant/sample ID 
-Barcode: PCR wellplate information from the sequencing center
-Multiplex_key: Details from the sequencing centre
-Number_of_reads: Number of sequencing reads from that sample
-Block: Spatial block in the common garden
-Line: Matriline from crossing design, there are 82 lines
-Germination_cohort: There were two cohorts of seeds planted. The first cohort was planted June 4th and the second cohort was planted July 2nd.
-Shape: Leaf shape genotype, determined visually once plant was matured. L=Homozygous lobed genotype, M= "Middle" Heterozygous genotype, H=Homozygous "Heart" whole genotype.
-FloweringDOY: Approximate day of the year (2021) when plants started flowering 
-Days_to_flowering: Number of days from planting to flowering for each plant
-Flowered_by_collection: Whether the plant was flowering when leaves were collected on September 1st, 2021. Y=Yes, N=No. Highly correlated with germination cohort.
-Leaf_area_cm2: Area of collected leaf in cm squared, determined with ImageJ
-Perimeter_cm: Perimeter of collected leaf in cm, determined with ImageJ
-Circularity: Circularity of collected leaf, calculated in ImageJ by outlining the main leaf and using using ImageJ formula of 4π(Area/Perimeter^2). Considers smoothness of the object. Value is between 0 and 1. 
-AR: Aspect ratio of collected leaf, calculated in ImageJ simultaneously with circularity.
-Round: Roundness of collected leaf, calculated in ImageJ simultaneously with circularity. Similar to aspect ratio, does not consider smoothness of perimeter as much as circularity.
-Solidity: Solidity of collected leaf, calculated in ImageJ simultaneously with circularity. Calculated by the area/convex hull area.


