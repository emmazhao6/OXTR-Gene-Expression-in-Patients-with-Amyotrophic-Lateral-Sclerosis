# EEB 5300: Final Project_Group 2 (updated on May/4/2021)<OXTR-Gene-Expression-in-Patients-with-Amyotrophic-Lateral-Sclerosis>
The entire process of the applied workflow should be documented and organized in Github, including the code itself and appropriate visualizations. This can also reference server paths as appropriate.
•	Overview and Motivation: Provide an overview of the project goals and the motivation.
Background of the research: Amyotrophic lateral sclerosis (ALS) is a non-demyelinating neurodegenerative disease in adults characterized by progressive muscle paralysis. Muscle paralysis is determined by the degeneration of motoneurons in the motor cortex brainstem and spinal cord. The ALS pathogenetic mechanisms are still unclear. The OXTR, a 7-transmembrane G protein-coupled receptor capable of binding to either Gi or Gq proteins, activates a set of signaling cascades, such as the MAPK, PKC, PLC, or CaMK pathways. The cellular response to OXT include of neurite outgrowth, cellular viability, and increased survival.RNA-sequencing (RNA-Seq) technology is a high-throughput sequencing technology which has developed rapidly in recent years. It can study gene function, gene structure and gene expression changes at the level of gene transcription and explore the genetic evidence supporting ALS treatment of peripheral nerve injury.

•	Related Work/Citations: If you are using public data, you would want to include relevant descriptions of  how the data was generated and cite this information.  Your analysis should be different from anything published on the data. If you are using data in your lab, ensure this data can be shared with your partner.  A full description of how this data was generated (study design, libraries, sequencing, etc) should be included.
•	https://pubmed.ncbi.nlm.nih.gov/27487029/ Brohawn, D. G., O'Brien, L. C., & Bennett, J. P., Jr (2016). RNAseq Analyses Identify Tumor Necrosis Factor-Mediated Inflammation as a Major Abnormality in ALS Spinal Cord. PloS one, 11(8), e0160520. https://doi.org/10.1371/journal.pone.0160520 "We isolated total RNA using postmortem cervical spinal section homogenates from 7 sporadic ALS and 8 neurologically healthy control samples. We prepared individual Illumina Total Stranded RNA-sequencing libraries for each sample. We sequenced each library with >50 million 2X150 sequencing reads on the NextSeq500. Our goal was to generate gene expression data for use in systems biology analyses to elucidate differences between the sALS and neurologically healthy control groups that may contribute to disease pathology."

•	Initial Questions: What questions are you trying to answer? How did these questions evolve over the course of the project? What new questions did you consider in the course of your analysis?
•	Research goal: To compare OXTR gene expression level to elucidate genetic differences between the ALS and neurologically healthy control groups that may contribute to disease pathology. Initial questions: What is the association between OXTR gene expression and ALS incidence?
•	Data: Location, Organization, Source
Illumina Total Stranded RNA sequencing reads from 15 postmortem cervical spinal sections (7 ALS and 8 healthy controls) 1 ILLUMINA (NextSeq 500) run: 71.8M spots, 21.5G bases, 9.7Gb downloads https://www.ncbi.nlm.nih.gov/sra/SRX1304782[accn] NCBI SRA RNAseq

•	Workflow/Analysis: What visualizations did you use to look at your data in different ways? What are the different statistical methods you considered? Why did you select the packages you did?
What visualizations did you use to look at your data in different ways? I am going to check the new gene, OXTR, to look at the association between OXTR and ALS risk by comparing the gene expression level in ALS patients and healthy control group. What are the different statistical methods you considered? I am going to use R, Linux, python which did not mention in the study. Why did you select the packages you did? The OXTR is a pain risk gene which related to etiology of neuropathy. By assessing and comparation of the gene expression level between ALS and healthy control group, we can understand the potential pathological mechanisms of ALS.

•	Discussion: What did you learn? What additional sequencing and/or analysis is necessary?  What questions remain?  What new insights were gained?
•	What did you learn? During this project preparation, I have learned that how to find the high throughput data and how to create file in github. Also I have learned how to download the RNAseq data from NCBI website. What additional sequencing and/or analysis is necessary? I am going to find the OXTR gene expression level from the data source of RNAseq. What questions remain? The author did not explore novel transcripts, indels, or selective exon usage specific to our sALS group in this dataset. And we may need to examine other gene expression level to see whether OXTR alone or together with other genes to influence the incidence of ALS. What new insights were gained? OUr next step is to investigate expression of smaller non-coding RNA’s, particularly microRNA’s, that regulate mRNA stability in the future study.




####################################
Final project Done! (4-27-2021)
1) Accessing the data using SRA-Toolkit:
We downloaded data from the NCBI. The SRA accessions are as follow:
ALS group: SRR2558714 & SRR2558715
Healthy control: SRR2558718 & SRR2558719
We download fastq files from SRA using sratoolkit 2.8.2.
We used the split_files, dump each read into separate files as our reads are paired-end.
The sequence files are in fastq format and compressed using gzip.
2) FASTQC
his step we perform a FASTQC which is helpful to see how the quality of the data has changed before and after trimming using Trimmomatic. To do this, we used the command-line of fastqc.
The fastq module load was version of 0.11.7
3) Quality Control Using Trimmomatic
We used Trimmomatic trim low-quality reads and adapter.
As our data is paired-end reads, we kept our fastq.gz separated in this procedure (SRR_1trim.fastq.gz & SRR_1un_trim.fastq.gz).
Instead of SE for single-end made, we used PE for paried-end mode based on the Trimmomatic manual.
ILLUMINACLIP: Truseq3-PE. Command searched for adapter sequence.
Slidingwindow: we used 4:15 scaned through the read, cutting the read when the average base quality in a 4 window frops below 15 which is matched with our quality scores.  
4) FASTQ before and after quality control
We used Cyberduck to view the ftml files.
5) Aligning reads to a genome using HISAT2
We used HISAT2 mapping our data against the hg19 human genome
We used wget command download the hg19 from ENSEMBL database.
Next, we created a genome index which is ordered in an easily navigable way with pointers to the information we seek within.
We used the hisat2-build module making a HISAT index file for the genome
We used pipe to send the results from HISAT2 to samtools to sort the sequences, convert them to binary format and compress them.  The resulting files were in BAM format.
Take one alignment summary of one single for example: 91.34% overall alignment rate.
Then we looked at the BAM files by sametools view –H SRR2558714.bam | head
6) Pairwise Differential Expression with Counts in R 
This step, we used R package DESeq2 to identify differentially expressed (DE) genes.
We used Cyberduck and downloaded the counts from xanadu.
Then we followed the tutorial to analyze our data. 
We did Histogram of the log scaled counts;Plot the shrunken log2 fold changes against the raw changes; we also get normalized, variance-stabilized transformed counts for visualization; We found top 50 log fold change genes. 

7)Next step: we are going to enlarge the sample size and normalize the count of each interested gene to analyze. 

The problem we have encountered:

1) How to download SRAtoolkit
Answer: "you should no need to download any software yourself for your efforts on the project.  SRAtoolkit is already installed on the cluster - you should use the same version referenced in he module load command in the script you used in class in the RNA-Seq tutorial - and you just copy that and change the accession number for your download. Also for very long running jobs, always use an sbatch script (just as we did in class) so that you don’t have to worry about your network connection staying open during the full download."

2)issues with Trimmomatic. The err reported that ILLUMINACLIP command not found, which is strange. I thought the Trimmomatic module was loaded then the ILLUMINACLIP should work automatically. 
Answer: "OK perfect - so both ILLUMINACLIP:TruSeq3-PE or ILLUMINACLIP:TruSeq2-PE are built in - you don’t need to create the file for them specifically
The example for PE data can be found here: http://www.usadellab.org/cms/index.php?page=trimmomatic"

3) Although we are using paired end data, we do not need merge pairs together, because we only can see one data set for each sample. Do we still use the PE version? 
Answer"Yes, you should still use the PE version - fastq-dump --split-files SRR#####; You will need to use the split-files command probably in order to get your Forward and Reverse sets split out. otherwise you might end up with just the one file representing both sets"

4) I finished count of each R1 and R2 separately. And I got raw data of each gene. However, I feel like we should merge them before doing the count? 
Answer"You want to keep each library separate through the process - you will QC them, then map them as pairs, and once you have this - you will end up with one alignment file per library; I would use R1 and R2 in a single run for each library (but still as separate files in the run) - the numbers should indeed be about the same but in order to have the correct format for the BAM; the manual for HISAT2 is here: http://daehwankimlab.github.io/hisat2/manual/ and you can indicate each read pair with -1 and -2 "






