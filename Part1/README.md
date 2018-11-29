# Obtaining and Filtering Data

## Downloading Raw Sequence Data

For this study, two random samples from each population will be downloaded. Their respective accession numbers are as follows:

* **Urban Italians**
	* Subject 8 - SRR1931178
	* Subject 11 - SRR1930248

* **Hadza Hunter-Gatherers**
	* Subject 19 - SRR1930132
	* Subject 26 - SRR1930143

To download each sample data set:
```wget ftp://ftp-trace.ncbi.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR193/SRR1931178/SRR1931178.sra```
```wget ftp://ftp-trace.ncbi.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR193/SRR1930248/SRR1930248.sra```
```wget ftp://ftp-trace.ncbi.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR193/SRR1930132/SRR1930132.sra```
```wget ftp://ftp-trace.ncbi.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR193/SRR1930143/SRR1930143.sra```

Each SRA data file downloaded must be coverted into the FASTQ format.  This can be done with the following command:
```/SRAToolKit/bin/fastq-dump --split-files <datafile.sra>``` 

Since the data contains paired-end reads, the `--split-files` argument dumps each read into separate files (R1 and R2).

Once all of the sequencing reads for each sample is downloaded and converted into FASTQ format, the quality can be assessed.

## Assessing the Quality of Reads

FastQC is 
