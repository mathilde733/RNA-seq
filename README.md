# **Project: _Toxoplasma gondii_ infection in the _Mus musculus_ Model, Immune response and differentially gene expression analysis**

This project is a part of the study conducted by [*Singhania et al, 2019, Transcriptional profiling unveils type I and II interferon networks in blood and tissues across diseases*](https://www.nature.com/articles/s41467-019-10601-6).

_Toxoplasma gondii_ is a widespread intracellular protozoan parasite capable of infecting most warm-blooded animals, including humans and rodents. The infection primarily targets various tissues, including the lungs and blood, where it can cause systemic and localized effects. This parasite is notable for its complex life cycle, which is divided in sexual and asexual phases. This life cycle contains intermediary hosts such as birds or any mammals, except felines that are considered as definitive host of this parasite. The infection caused by _Toxoplasma gondii_ is called the toxoplasmosis, which is typically asymptomatic in healthy individuals, but poses significant risks to immunocompromised persons or pregnant women. Additionaly, according to previous studies, it appears that this parasite can be linked to human neuropsychiatric disorders. 

By integrating several bioinformatics tools, this project aims to identify the differential expression of immune-related genes in infected versus control tissues, focusing on tissue-specific immune responses.  

The samples are RNA sequencing data of mice's blood and lungs that were infected or not with _Toxoplasma gondii_, as described on the table below.

| Code        | Condition | Origin of Sample |              | Code        | Condition | Origin of Sample |
|-------------|-----------|------------------|--------------|-------------|-----------|------------------|
| SRR7821937  | Control   | Lung             |              | SRR7821968  | Control   | Blood            |
| SRR7821938  | Control   | Lung             |              | SRR7821969  | Control   | Blood            |
| SRR7821939  | Control   | Lung             |              | SRR7821970  | Control   | Blood            |
| SRR7821918  | Infected  | Lung             |              | SRR7821949  | Infected  | Blood            |
| SRR7821919  | Infected  | Lung             |              | SRR7821950  | Infected  | Blood            |
| SRR7821920  | Infected  | Lung             |              | SRR7821951  | Infected  | Blood            |
| SRR7821921  | Infected  | Lung             |              | SRR7821952  | Infected  | Blood            |
| SRR7821922  | Infected  | Lung             |              | SRR7821953  | Infected  | Blood            |


## **First step: Quality control**

The quality control script can be found on [01_QC_trim_script](https://github.com/mathilde733/RNA-seq/blob/main/01_QC_trim_script). The MultiQC script can be found on [02_multiQC_script](https://github.com/mathilde733/RNA-seq/blob/main/02_multiQC_script). 
The tools used during this step are: FastQC, Trimmomatic and MultiQC. The trimming was not mandatory but can ensure the quality of the reads. Alternative can be used such as Fastp or CutAdapt.

## **Second step: Mapping**

The corresponding script can be found on [03_mapping_script](https://github.com/mathilde733/RNA-seq/blob/main/03_mapping_script) and passed through different steps such as building the index and mapping the reads to the reference genome, using the [*reference genome*](https://ftp.ensembl.org/pub/release-113/fasta/mus_musculus/dna/Mus_musculus.GRCm39.dna.primary_assembly.fa.gz) and the [*annotation file*](https://ftp.ensembl.org/pub/release-113/gtf/mus_musculus/Mus_musculus.GRCm39.113.gtf.gz).
The tools used during this step are: HISAT2, Samtools and optionally Integrative Genomics Viewer (IGV) to visualize the genome. 

## **Third step: Count the number of reads per gene**

The corresponding script can be found on [04_counts_script](https://github.com/mathilde733/RNA-seq/blob/main/04_counts_script) and used the same annotation file as for the mapping, the [*annotation file*](https://ftp.ensembl.org/pub/release-113/gtf/mus_musculus/Mus_musculus.GRCm39.113.gtf.gz).
The tool used during this step is featureCounts.

This is a key step because the table that you will obtained with featureCounts will be used for the R analysis, e.g., to create the DESeq2 object. For further analysis, you have to download the featureCounts table. 

## **Final step: in R** 

### **Exploratory data analysis** 

The corresponding script can be found on [05_explanatory_analysis_R](https://github.com/mathilde733/RNA-seq/blob/main/05_explanatory_analysis_R) and passed through different steps such as the creation of a DESeq object and then the visualization of gene expression patterns using several tools. The clustering of the groups can be visualize with a Principal Component Analysis (PCA), a Principal Coordinates Analysis (PCoA) and a Heat Map. 
The tool used during this step is R with different packages that have to be downloaded before running the analysis: library(DESeq2), library(clusterProfiler), library(pheatmap), library(vegan) and library(ggplot2). 

### **Differential expression and overrepresentation analysis** 

The corresponding script of the **differential expression analysis** can be found on [06_differential_expression_R](https://github.com/mathilde733/RNA-seq/blob/main/06_differential_expression_R) and aims to compare which are the differentially expressed genes between the conditions based on tissues. The threshold is here based on 0.05 (5%) but you can use a lower threshold to have less significant genes. Based on these results, you can calculate the number of up- and down-regulated genes in the samples. Visualization can be done with Volcano Plots. 
The tool used during this step is R using the packages: library(biomaRt), library(org.Mm.eg.db) and library(EnhancedVolcano).

The outliers observed on the Volcano Plots were analysed with Boxplots to have a graphical representation of their distributions within the groups. You can simply run the analysis using any genes that are associated with its own ENSEMBL gene IDs that you can find on the [*website*](https://www.ensembl.org/index.html).

The corresponding script of the **overrepresentation analysis** can be found on [07_overrepresentation_R](https://github.com/mathilde733/RNA-seq/blob/main/07_overrepresentation_R). The overrepresentation analysis provides Gene Ontology (GO) terms that can be visualize using dot plots and bar plots. Again, be careful of what options you specify in R. For the purpose of this project, I selected "BP - Biological Processes" as subontology but you can use another one. 
The tool used during this step is R using packages: library(clusterProfiler) and library(org.Mm.eg.db)

## **On this repository, you will also find:**
- the FastQC files
- the [MultiQC report](https://github.com/mathilde733/RNA-seq/blob/main/multiqc_report.html)
- the [featureCounts table](https://github.com/mathilde733/RNA-seq/blob/main/reformatted_counts.txt) 




