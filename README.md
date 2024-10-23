# Epigenomicsc- Chip-seq-Analysis
ChIP-sequencing, also known as ChIP-seq, is a method used to analyze protein interactions with DNA. ChIP-seq combines chromatin immunoprecipitation (ChIP) with massively parallel DNA sequencing to identify the binding sites of DNA-associated proteins. The application of next-generation sequencing (NGS) to ChIP has revealed insights into gene regulation events that play a role in various diseases and biological pathways, such as development and cancer progression. ChIP-Seq enables a thorough examination of the interactions between proteins and nucleic acids on a genome-wide scale.

# Resources Used

| Tool/Resource                       | Command/Link                                                                  |
|-------------------------------------|-------------------------------------------------------------------------------|
| **FastQC**                          | [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)          |
| **Cutadapt**                        | `cutadapt -o output.fastq input.fastq`                                        |
| **Bowtie**                          | [Bowtie](http://bowtie-bio.sourceforge.net/index.shtml)                       |
| **Samtools**                        | [Samtools](http://samtools.sourceforge.net/)                                  |
| **BEDTools**                        | [BEDTools](http://code.google.com/p/bedtools/)                                |
| **UCSC Tools**                     | [UCSC Tools](http://hgdownload.cse.ucsc.edu/admin/exe/)                        |
| **IGV Genome Browser**              | [IGV](http://www.broadinstitute.org/igv/)                                     |
| **MACS**                            | [MACS](http://liulab.dfci.harvard.edu/MACS/index.html)                        |
| **ChipAnnotator**                  | In-house ArrayGen Proprietary tool for chip annotation                         |
| **MEME**                            | [MEME](http://meme.sdsc.edu/meme/cgi-bin/meme.cgi)                            |
| **TOMTOM**                          | [TOMTOM](https://meme-suite.org/meme/tools/tomtom)                           |
| **DAVID**                           | [DAVID](http://david.abcc.ncifcrf.gov)                                        |
| **GOstat**                          | [GOstat](http://meme.sdsc.edu/meme/cgi-bin/tomtom.cgi)                        |
| **UCSC**                            | [UCSC](https://genome.ucsc.edu/)                                              |
| **EBI SRA**                         | [EBI SRA](http://www.ebi.ac.uk/arrayexpress/experiments/)                     |


## Overview of a bioinformatics workflow that involves downloading sequencing data, performing quality control, aligning reads to a reference genome, and converting SAM files to BAM files.

### Workflow Summary

<img width="629" alt="chip-seq workflow" src="https://github.com/user-attachments/assets/931a049b-d91c-4acd-aaaa-e137571cf659">


1. **Downloading Sequencing Data:**
   - Control and treated sample data are downloaded using `wget` commands.
   - Reference genome files for the human genome (chromosomes 1 to Y) are also downloaded.

   ```bash
   wget -c ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR708/009/SRR7080719/SRR7080719.fastq.gz
   wget -c ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR708/008/SRR7080718/SRR7080718.fastq.gz
   wget -c http://hgdownload.soe.ucsc.edu/goldenPath/hg38/chromosomes/chr1.fa.gz
   wget -c http://hgdownload.soe.ucsc.edu/goldenPath/hg38/chromosomes/chrY.fa.gz
   ```

2. **Decompressing Files:**
   - All downloaded `.gz` files are unzipped using `gunzip`.

   ```bash
   gunzip *.gz
   ```

3. **Quality Control with FastQC:**
   - FastQC is utilized to assess the quality of the sequencing reads. It provides an interactive report on various metrics (like sequence quality, GC content, etc.).
   - To run FastQC on the downloaded FastQ files:

   ```bash
   fastqc Control_SRR7080719.fastq treated_SRR7080718.fastq
   ```

4. **Alignment with Bowtie 2:**
   - The reference genome is indexed using Bowtie 2 before aligning the sequencing reads.
   - Indexing the reference genome:

   ```bash
   bowtie2-build human_Genome.fa human_Genome
   ```

   - Aligning the control and treated reads to the reference genome:

   ```bash
   bowtie2 --very-sensitive -x human_Genome -q Control_SRR7080719.fastq -p 2 > Control_SRR7080719.sam
   bowtie2 --very-sensitive -x human_Genome -q treated_SRR7080718.fastq -p 2 > treated_SRR7080718.sam
   ```

5. **Converting SAM to BAM:**
   - The output from Bowtie 2 in SAM format is converted to BAM format using `samtools`.

   ```bash
   samtools view -bS Control_SRR7080719.sam > Control_SRR7080719.bam
   samtools view -bS treated_SRR7080718.sam > treated_SRR7080718.bam
   ```

### Additional Information
- **FASTQ Format:** The format includes sequences and their corresponding quality scores, essential for evaluating sequencing quality.
  
- **Quality Scores:** The quality scores are encoded using the Phred scale, indicating the probability of an incorrect base call.

- **BAM File Format:** BAM files are compressed binary versions of SAM files, allowing for efficient storage and processing.

- **Bowtie 2 Options:** It’s beneficial to explore the various options provided by Bowtie 2, such as setting the number of threads for faster processing.

