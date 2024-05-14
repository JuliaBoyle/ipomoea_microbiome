# Genetic architecture of heritable leaf microbes

This repository includes code and data for analyzing Ipomoea hederacea's leaf microbiome and identifying heritable microbes. Access this dataset on Dryad https://doi.org/10.5061/dryad.rbnzs7hjz(opens in new window)

## 1. Background
Overall goal: Using Ipomoea hederacea, Ivy-leaved morning glory, we generated matrilines differing in quantitative genetic variation and leaf shape, which is controlled by a single Mendelian locus. We then investigated the relative roles of Mendelian and quantitative genetic variation in structuring the leaf microbiome in a common garden, and how these two sources of genetic variation contributed to microbe heritability.

Breeding design: We used seeds derived from a cross by Campitelli and Stinchcombe (2013), where individuals from the two alternate homozygous leaf shape phenotypes (i.e., fully lobed or whole) were collected from North Carolina, USA, selfed for seven generations to generate homozygous parents (P1), and then crossed with each other (Figure 1). A single F1 was allowed to self-fertilize, producing F2 progeny. We scored F2 plants for leaf shape, and allowed them to self-fertilize; we refer to all the selfed progeny of an individual F2 plant as a “matriline.”  We used F3 seeds, set by F2 plants we had identified as heterozygous for leaf shape, as our experimental seeds.

Data collection: In 2021, we planted a total of 240 I. hederacea seeds from 82 matrilines (2-3 seeds/matriline) in a common garden at Koffler Scientific Reserve (www.ksr.utoronto.ca(opens in new window)) in Ontario, Canada. We scarified seeds then planted them in a greenhouse in peat pots containing soil from the field. We planted the first cohort on June 4th and a second cohort on July 2nd, because of poor germination in the first cohort. After the majority of plants in a given cohort germinated (approximately one week), we transplanted the pots into the field. The 218 surviving plants consisted of 83 individuals from the first cohort, and 135 individuals from the second cohort. On September 1st, we collected similarly-sized leaves within one foot from the ground by cutting them with sterilized scissors into a sterile plastic bag, after which they were stored in a -80°C freezer until DNA extraction.

Sequencing and analysis: We performed extractions using the whole leaves with QIAGEN DNeasy® PowerSoil® Pro Kits; both epiphytic and endophytic microbes were extracted. We sent samples to Génome Québec (Montréal, Canada) for Illumina MiSeq PE 250bp 16S rRNA gene amplicon sequencing on the conserved hypervariable V4 region (primer pair 515F-806R). Raw microbiome sequence data is available at NCBI’s Sequence Read Archive under the BioProject accession number PRJNA1107536. We used Quantitative Insights Into Microbial Ecology 2 (QIIME2) v.2022.2 to trim the sequences for quality and we denoised the sequences with DADA2. Using QIIME2, we removed amplicon sequence variants (ASVs) that had fewer than 10 reads across all samples, and assigned taxonomy using the ‘sklearn’ feature classifier and Greengenes 16S rRNA gene V4 region reference, then filtered out reads assigned as cyanobacteria and mitochondria to remove plant DNA. Finally, we constructed a phylogeny using QIIME2’s MAFFT and FastTree 2 to obtain a rooted tree.

## 2. Description of the data and file structure
### Code

The file "Leaf shape microbiome.rmd" contains all R code necessary to reproduce the analysis, commented throughout. Open the R markdown file using R and  R Studio.

### Metadata

The file "ipomoea-taxa-metadata-allnumbers.csv" contains the full metadata of the experiment, including all plants that survived and died during the experiment. More information on the column meanings and interpretations are included in section 3. 

The file "ipomoea_metadata.tsv" contains the metadata of only plants that survived, and are included in the analysis. More information on the column meanings and interpretations are included in section 3. 

### Data

The file "table-try3-10readsmin-filtered-cyano-mito-glory.qza" is an output from QIIME2 and contains the feature table/ASV table. The table has been filtered to 10 reads minimum per ASV. Cyanobacteria and mitochondria have been filtered out to remove plant and chloroplast DNA. Columns represent a single sampled leaf, with the number corresponding to the unique plant ID. Rows are the amplicon sequence variants and their unique name assigned by QIIME2. Amplicon sequence read abundance is contained in the table.

The file "table-try3-10readsmin-filtered-cyano-mito.csv" is a csv file version of the QIIME2 feature table, transposed. The table has been filtered to 10 reads minimum per ASV. Cyanobacteria and mitochondria have been filtered out to remove plant and chloroplast DNA. Rows represent a single sampled leaf, with the number corresponding to the unique plant ID. Columns are the amplicon sequence variants and their unique name assigned by QIIME2. Amplicon sequence read abundance is contained in the table.

The file "rooted-tree-try3-glory.qza" is an output from QIIME2 and contains the rooted tree of taxa in the sequencing data. Tip labels correspond to amplicon sequence variant unique identifiers. The tree includes branch lengths and node labels indicating confidence in the node.

The file "taxonomy-try3-glory.qza" is an output from QIIME2 and contains the assigned Greengenes taxonomy linking ASV to classification. Here, rows represent amplicon sequence variants labelled with their unique identifier. Columns include the assigned taxonomy at different resolutions including Kingdom, Phylum, Class, Order, Family, Genus, and Species. NAs mean that amplicon sequence variant was unable to be assigned a confident taxonomy at that depth.

The file "taxonomy_try3.csv" is a csv file version of the taxonomy classification. Here, each row is an amplicon sequence variant, with the unique identifier name indicated in the Feature.ID column. The Taxon column contains the assigned taxonomy as a single string. Here k__ indicates Kingdom, p__ indicates Phylum, c__ indicates Class, o__ indicates Order, f__ indicates Family, g__ indicates Genus, and s__ indicates species. NAs mean that amplicon sequence variant was unable to be assigned a confident taxonomy at that depth. The last column is named Confidence and is the confidence with which taxonomy was assigned by Greengenes; in other words, how well the DNA sequence matches the assigned taxon.

The file "phygloryCLRGenfeaturetable.csv" was created in R (code to create it is in the code .rmd file. It contains the abundances of genera found in at least 30% of samples, and this is the manually-transposed version with samples as rows. Each row represents a single sampled leaf, with the number corresponding to the unique plant ID. Each column name is a genus. The table is filled with read abundance counts. If you are following the analysis, this file is next read back into R, then (following the code) centered log ratio transformed for downstream analysis.

## 3. Metadata explained in more detail
### Rows

Rows and row names indicate the unique sample ID of the plant. One leaf was sampled from each unique plant.

### Columns

Barcode: PCR wellplate information from the sequencing center, can be ignored for analysis.

Multiplex_key: Details from the sequencing centre on how the samples were multiplexed. Can be ignored for analysis.

Number_of_reads: Total number of sequencing reads in that sample.

Block: The location of the plant in the common garden experiment, the experiment had three spatial blocks.

Line: Matriline of the plant from crossing design. There are 82 lines, with unique number identifiers.

Germination_cohort: There were two cohorts of seeds planted. The first cohort was planted June 4th and the second cohort was planted July 2nd. This column indicates which cohort that plant was planted in.

Shape: Leaf shape genotype of the plant, determined visually once plant was matured. L=Homozygous lobed genotype, M= "Middle" Heterozygous genotype, H=Homozygous "Heart" whole genotype.

FloweringDOY: Approximate day of the year (2021) when the plant started flowering. For example, January 1st would equal 1.

Days_to_flowering: Number of days between planting to flowering for each plant

Flowered_by_collection: Whether the plant was flowering when leaves were collected on September 1st, 2021. Y=Yes, N=No. Highly correlated with germination cohort.

Leaf_area_cm2: Area of collected leaf in centimeters squared, determined with ImageJ. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

Perimeter_cm: Perimeter of collected leaf in centimeters, determined with ImageJ. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

Circularity: Circularity of collected leaf, calculated in ImageJ by outlining the main leaf and using using ImageJ formula of 4π(Area/Perimeter^2). Considers smoothness of the object. Value is between 0 and 1. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

AR: Aspect ratio of collected leaf, (length of major axis/length of minor axis) calculated in ImageJ. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

Round: Roundness of collected leaf, calculated in ImageJ simultaneously with circularity. Similar to aspect ratio, it does not consider the smoothness of perimeter as much as circularity. Calculated by 4*area/(π*major_axis^2), or the inverse of the aspect ratio. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

Solidity: Solidity of collected leaf, calculated in ImageJ simultaneously with circularity. Calculated by the area/convex hull area. Photos of the leaves included a 1x1 centimeter grid, used to calibrate ImageJ distances. More information can be found at https://imagej.net/ij/docs/menus/analyze.html(opens in new window)

## 4. How to use this data, code/software
Download all files, and use R and R Studio to run the Rmarkdown (.Rmd) file. Some files with the file type .qza can be used or examined in QIIME2, often accessed by command line or with https://view.qiime2.org/(opens in new window).

R session info:

R version 4.2.0 (2022-04-22 ucrt)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 10 x64 (build 22631)

Matrix products: default

locale:
[1] LC_COLLATE=English_Canada.utf8  LC_CTYPE=English_Canada.utf8   
[3] LC_MONETARY=English_Canada.utf8 LC_NUMERIC=C                   
[5] LC_TIME=English_Canada.utf8    

attached base packages:
[1] parallel  stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 [1] mdthemes_0.1.0              picante_1.8.2               nlme_3.1-157               
 [4] viridis_0.6.4               viridisLite_0.4.2           patchwork_1.1.3            
 [7] speedyseq_0.5.3.9018        microshades_1.10            CoDaSeq_0.99.6             
[10] ALDEx2_1.28.1               phyloseqCompanion_1.1       rlang_1.1.1                
[13] magrittr_2.0.3              GUniFrac_1.8                doParallel_1.0.17          
[16] iterators_1.0.14            foreach_1.5.2               data.table_1.14.2          
[19] zCompositions_1.4.1         truncnorm_1.0-9             NADA_1.6-1.1               
[22] survival_3.3-1              MASS_7.3-56                 compositions_2.0-6         
[25] insight_0.19.6              cowplot_1.1.1               randomcoloR_1.1.0.1        
[28] RColorBrewer_1.1-3          vegan_2.6-4                 lattice_0.20-45            
[31] permute_0.9-7               car_3.1-2                   carData_3.0-5              
[34] lubridate_1.9.3             forcats_1.0.0               stringr_1.5.1              
[37] purrr_1.0.1                 readr_2.1.4                 tidyr_1.3.0                
[40] tibble_3.2.1                tidyverse_2.0.0             plyr_1.8.7                 
[43] ANCOMBC_1.6.0               qiime2R_0.99.6              microbiomeutilities_1.00.16
[46] dplyr_1.1.2                 microbiome_1.18.0           phyloseq_1.40.0            
[49] MicrobeR_0.3.2              philr_1.22.0                plotly_4.10.3              
[52] ggplot2_3.5.1               adespatial_0.3-23           ranacapa_0.1.0             
[55] microbiomeSeq_0.1           ape_5.7-1                   pander_0.6.5               
[58] lmerTest_3.1-3              lme4_1.1-35.1               Matrix_1.5-1               

loaded via a namespace (and not attached):
  [1] Hmisc_5.1-1                 class_7.3-20                ps_1.7.5                   
  [4] glmnet_4.1-8                crayon_1.5.2                V8_4.4.0                   
  [7] rbibutils_2.2.16            rhdf5filters_1.7.0          backports_1.4.1            
 [10] fBasics_4032.96             huge_1.3.5                  XVector_0.35.0             
 [13] performance_0.10.8          nloptr_2.0.3                callr_3.7.3                
 [16] phylobase_0.8.10            BiocParallel_1.29.21        bit64_4.0.5                
 [19] glue_1.6.2                  loo_2.6.0                   sjPlot_2.8.15              
 [22] pheatmap_1.0.12             rstan_2.32.3                processx_3.8.2             
 [25] BiocGenerics_0.42.0         classInt_0.4-10             VGAM_1.1-9                 
 [28] SummarizedExperiment_1.26.1 tidyselect_1.2.0            XML_3.99-0.15              
 [31] sf_1.0-14                   ggpubr_0.6.0                sjmisc_2.8.9               
 [34] xtable_1.8-4                spData_2.3.0                evaluate_0.23              
 [37] Rdpack_2.6                  cli_3.6.1                   zlibbioc_1.41.0            
 [40] pulsar_0.3.11               rstudioapi_0.15.0           sp_2.1-1                   
 [43] rpart_4.1.16                fastmatch_1.1-4             sjlabelled_1.2.0           
 [46] GLMMadaptive_0.9-1          treeio_1.20.0               shiny_1.7.5.1              
 [49] xfun_0.39                   clue_0.3-65                 inline_0.3.19              
 [52] pkgbuild_1.4.2              multtest_2.51.0             cluster_2.1.3              
 [55] stable_1.1.6                biomformat_1.24.0           ggrepel_0.9.4              
 [58] stabledist_0.7-1            Biostrings_2.63.3           png_0.1-8                  
 [61] withr_2.5.2                 bitops_1.0-7                RcppZiggurat_0.1.6         
 [64] e1071_1.7-13                coda_0.19-4                 pillar_1.9.0               
 [67] RcppParallel_5.1.7          cachem_1.0.8                fs_1.6.3                   
 [70] rmutil_1.1.10               vctrs_0.6.3                 ellipsis_0.3.2             
 [73] generics_0.1.3              tools_4.2.0                 foreign_0.8-82             
 [76] rncl_0.8.7                  munsell_0.5.0               DelayedArray_0.21.2        
 [79] emmeans_1.8.9               proxy_0.4-27                fastmap_1.1.1              
 [82] compiler_4.2.0              abind_1.4-5                 httpuv_1.6.12              
 [85] GenomeInfoDbData_1.2.8      gridExtra_2.3               ggnewscale_0.4.9           
 [88] QuickJSR_1.0.7              deldir_1.0-9                utf8_1.2.3                 
 [91] later_1.3.1                 wk_0.9.0                    jsonlite_1.8.0             
 [94] scales_1.3.0                statip_0.2.3                tidytree_0.4.5             
 [97] estimability_1.4.1          lazyeval_0.2.2              promises_1.2.1             
[100] latticeExtra_0.6-30         checkmate_2.3.0             rmarkdown_2.25             
[103] statmod_1.5.0               Rtsne_0.16                  Biobase_2.55.2             
[106] igraph_1.5.0                numDeriv_2016.8-1.1         yaml_2.3.7                 
[109] htmltools_0.5.5             memoise_2.0.1               IRanges_2.29.1             
[112] quadprog_1.5-8              digest_0.6.32               mime_0.12                  
[115] bayestestR_0.13.1           units_0.8-4                 RSQLite_2.3.3              
[118] Rfast_2.1.0                 yulab.utils_0.1.0           microViz_0.12.1            
[121] blob_1.2.4                  S4Vectors_0.33.17           RNeXML_2.4.11              
[124] modeest_2.4.0               splines_4.2.0               Formula_1.2-5              
[127] DECIPHER_2.24.0             Rhdf5lib_1.17.3             RCurl_1.98-1.6             
[130] broom_1.0.5                 hms_1.1.3                   modelr_0.1.11              
[133] rhdf5_2.39.6                colorspace_2.0-3            base64enc_0.1-3            
[136] GenomicRanges_1.47.6        shape_1.4.6                 aplot_0.2.2                
[139] nnet_7.3-17                 Rcpp_1.0.11                 mvtnorm_1.2-3              
[142] fansi_1.0.4                 tzdb_0.4.0                  R6_2.5.1                   
[145] grid_4.2.0                  lifecycle_1.0.4             StanHeaders_2.26.28        
[148] adegraphics_1.0-21          curl_5.1.0                  ggsignif_0.6.4.9000        
[151] adegenet_2.1.10             minqa_1.2.6                 phangorn_2.11.1            
[154] robustbase_0.99-0           htmlwidgets_1.6.2           timechange_0.2.0           
[157] gridGraphics_0.5-1          SpiecEasi_1.1.2             timeSeries_4031.107        
[160] mgcv_1.8-40                 htmlTable_2.4.2             gghalves_0.1.4             
[163] tensorA_0.36.2              codetools_0.2-18            matrixStats_1.1.0          
[166] rtk_0.2.6.1                 prettyunits_1.2.0           GenomeInfoDb_1.32.1        
[169] gtable_0.3.4                DBI_1.1.3                   bayesm_3.1-6               
[172] stats4_4.2.0                adephylo_1.1-16             ggfun_0.1.3                
[175] httr_1.4.7                  KernSmooth_2.23-20          stringi_1.7.6              
[178] progress_1.2.2              reshape2_1.4.4              uuid_1.1-1                 
[181] spatial_7.3-15              spdep_1.2-8                 timeDate_4022.108          
[184] ggtree_3.4.4                DT_0.30                     seqinr_4.2-30              
[187] xml2_1.3.5                  boot_1.3-28                 ggeffects_1.3.2            
[190] s2_1.1.4                    interp_1.1-4                ade4_1.7-19                
[193] ggplotify_0.1.2             DEoptimR_1.1-3              bit_4.0.5                  
[196] MatrixGenerics_1.8.0        sjstats_0.18.2              ggside_0.3.1               
[199] jpeg_0.1-10                 pkgconfig_2.0.3             rstatix_0.7.2              
[202] knitr_1.45   

## 5. Sharing/access information
Code and data is also publically available on GitHub (https://github.com/JuliaBoyle/ipomoea_microbiome(opens in new window)

The raw microbiome sequence data is available at NCBI’s Sequence Read Archive under the BioProject accession number PRJNA1107536.
