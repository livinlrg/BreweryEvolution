# Brewery Evolution

These are the scripts and data that were used to generate the figures presented in:

**Genomic stability and adaptation of beer brewing yeasts during serial repitching in the brewery**  
*Christopher R. L. Large, Noah A Hanson, Andreas Tsouris, Omar Abou Saada, Jirasin Koonthongkaew, Yoichi Toyokawa, Tom Schmidlin, Daniela A Moreno-Habel, Hal McConnellogue, Richard Preiss, Hiroshi Takagi, Joseph Schacherer, Maitreya J Dunham*

doi: https://doi.org/10.1101/2020.06.26.166157

## Table of Contents
1. Data
- (A) Plate Reader
- (B) Settling Assay
- (C) Sensory Data
2. Scripts
- (A) Alignment and Variant Calling
- (B) Copy Number Analysis
- (C) Allele Frequency
- (D) Phylogenomics
- (E) Growth Rate
- (F) Sensory Analysis
- (G) Assembly Polishing

# Data
## (A) Plate Reader
Raw data from the plate reader from the experiments documented in the manuscript are avaliable. They include data from the wort conditions as well as unmentioned experiments that utilized starting ethanol ranging from 0 to 10% (v/v).  
  
The files are:  
-  2018_05_09_PDB_EtOh_Formatted.csv  
-  2018_06_05_TestForEthanolInWort.csv  
-  2018_06_08_GrowthCurve_YEPD_WORT.csv  
-  2018_06_13_TestForEthanolInWort.csv  

## (B) Settling Assay  
The raw images associated with the settling assay experiments are avaliable. The two replicates are split into two directories:
- 20180621
- 20180524

## (C) Sensory Analysis
The reformatted responses from the sensory panel done at HomebrewCon 2018 are avaliable at:
- Beer_ScoreSheet_Reformatted.txt

# Scripts
Please note that in many instances, the scripts outlined here are written for my computing cluster and will require some retooling if they are to be adapted for other uses. Please bear with my occasional hard coding of directories. Hopefully, they can serve as inspiration for further studies. 

## (A) Alignment and Variant Calling
###### runSeqAlignVariantCall_20200504.sh
1. The reads mentioned in the paper were aligned to the SacCer3 genome from SGD. They were then preprocessed with GATK and picard tools
2. Variant calls for the ancestor were generated using Samtools and Freebayes.
3. With the ancestor variant calls in hand, the evolved samples were called with LoFreq in paired mode and the variant calls from Samtools and Freebayes were compared to the ancestor.
4. The variants were filetered and annotated using: yeast_annotation_chris_edits_20170925.py
5. The variants found in the clones were comapred using MakeOverlapMatrix.R to generate an overlap matrix.

## (B) Copy Number Analysis
######  runCNScripts_Populations_20200310.sh
1. The alignments from part (A) were measured for their genome wide depth of coverage using GATK/2. Please note that there is a new tool in GATK/4 that serves the same function. 
2. 1000-bp sliding windows of coverage were generated with the IGVtools command line implementation. Two iterations of this were done to generate a file with the filtering option of MAPQ > 30 and MAPQ > 0.
3. These files were then compared using: wigNormalizedToAverageReadDepth_MapQ_ForPlot.py
4. Plotting of invdividual sampels was done with: PlotCopyNumber_OneSample_20200106.R
5. Furthering plotting, comparing multiple samples for the copy number figure were done with: Copy_Number_Plot_Text_20200619.R and Copy_Number_Population_AllChrom_20200609.R

## (C) Allele Frequency
###### runVariantCall_GATK_Populations_20200310.sh
1. From the alignments generated in part (A), GATK variant calling using haplotype caller was used.
2. The variants were filtered to just include the highest confident SNPs using GATK. The SNPs were then processed into a table format.
3. Using a java script (VaraiantTableParse_GATK_BAF_20190206.jar) the table of SNPs were convereted into a ratio of reference or alternate. 
4. The output files are then plotted using a variety of scripts: AlleleFrequency_Population_AllChrom_20200429.R, Allele_Frequency_Clones_Plot_2020428.R, and Allele_Frequency_Plot_20200619.R






