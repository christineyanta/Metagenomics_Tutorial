# Read Assembly and Mapping

## Read Assembly

At this point, you should have all the quality-filtered .fastq files for all four samples under analysis (there should be 8 in total, two for each sample). For simplicity, the data for each population should be seperated into different directories:

* Italian Population:

	```$ ls /MetagenomicsTutorial/ItalianData/*fastq```

	The output should appear as:

	```(bash)
	Subject8.R1.fq
	Subject8.R2.fq
	Subject11.R1.fq
	Subject11.R2.fq
	```
* Hadza Population:

	```$ ls /MetagenomicsTutorial/HadzaData/*fastq```

	The output should appear as:
	```(bash)
	Subject19.R1.fq
	Subject19.R2.fq
	Subject26.R1.fq
	Subject26.R2.fq
	```
The next step is to perform an assembly on the read data to form contigs. For this tutorial, the two samples from each population will be combined. Generally, each sample is assembled separately; however to make things simpler due to limited resources, the two samples from each population will be combined in the assembly process. Therefore, an two assemblies will be created: Italian Assembly and Hadza Assembly.

The program which we will use to assemble the contigs within the read files is [Megahit](https://github.com/voutcn/megahit).  This de novo assembler is able to assemble large and complex metagenomics in an efficient manner. An alternative program to Megahit is [metaSpades](https://www.ncbi.nlm.nih.gov/pubmed/28298430).

Therefore, to assemble the Italian sequence data:

1. We want to create two environment variables to classify all the R1 files from R2.

```(bash)
$ R1s=`ls /MetagenomicsTutorial/ItalianData/*R1.fq | python -c 'import sys; print(",".join([x.strip() for x in sys.stdin.readlines()]))
$ R2s=`ls /MetagenomicsTutorial/ItalianData/*R2.fq | python -c 'import sys; print(",".join([x.strip() for x in sys.stdin.readlines()]))
```

2. Ensure the environmental variables are set properly. Your output should appear as follows:

```(bash)
$ echo $R1s
Subject8.R1.fq,Subject11.R1.fq
$ echo $R2s
Subject8.R2.fq,Subject11.R2.fq
```

3. Perform the assembly with Megahit:

```$ megahit -1 $R1s -2 $R2s --min-contig-len 1000 -m 0.85 -o Assembly/ -t 8```

The arguments are as follows:
* `-1` : Read1 Input File(s)
* `-2` : Read2 Input File(s)
* `--min-contig-len` : the minimum length for a contig to keep
* `-m 0.85` : use 85% of memory available
* `-o Assembly/` : output directory
* `-t 8` : use 8 threads

Once the assembly is finished, you can take a look at the statistics of the assembly performed:

```(bash)
$ tail Assembly/log
b'    [assembler.cpp             : 225]     Number of complex bubbles removed: 0, Time elapsed(sec): 0.235667'
b'    [assembler.cpp             : 243]     Number unitigs disconnected: 0, time: 0.031702'
b'    [assembler.cpp             : 265]     Unitigs removed in excessive pruning: 0, time: 0.003666'
b'    [assembler.cpp             : 317]     Number of local low depth unitigs removed: 547, complex bubbles removed: 29, time: 0.950630'
b'    [assembler.cpp             : 132]     Total length: 119418159, N50: 1586, Mean: 1095, number of contigs: 108969'
b'    [assembler.cpp             : 133]     Maximum length: 195196'
b'    [utils.h                   : 126]     Real: 232.0211\tuser: 724.6043\tsys: 117.9306\tmaxrss: 518266880'
--- [2018] Merging to output final contigs ---
--- [STAT] 27965 contigs, total 76168090 bp, min 1000 bp, max 195196 bp, avg 2724 bp, N50 3309 bp
--- [2018] ALL DONE. Time elapsed: 7377.960897 seconds ---
```

From this log file, we can examine the number of contigs, total length, average contig length and N50. Observe the data for yourself.

Repeat steps 1-3 for the Hadza data.


## Mapping

The next step in this tutorial will demonstrate how to map the reads to the contig assembly we just produced.

Bowtie2 is a sequence aligner that maps short reads to a large reference genome. This program is memory-efficient, allowing for quick analysis.

The first step towards aligning with Bowtie2 is to index the 'reference genome', or in this case our contigs:

```(bash)
$ mkdir Mapping
$ bowtie2-build contigs.fa Mapping/contigs
```

Then for each sample, you will need to map the reads to the indexed reference genome:

```(bash)
$ bowtie2 --threads 8 -x Mapping/contigs -1 Sample.R1.fq -2 Sample.R2.fq -S 04_MAPPING/Sample.sam
```

The first line simply maps the reads to the contigs generated, creating a SAM file.
* Arguments Defined:
	* `--thread 8` : 8 threads were used to complete the command
	* `-x Mapping/contigs` : the indexed contigs (reference genome)
	* `-1 Sample.R1.fq -2 Sample.R2.fq` : paired-end read data
	* `-S Mapping/Sample.sam` : generate a SAM file with the mapping results

When this command is completed, it will output statistics from the mapping, such as:

```(bash)
3242487 reads; of these:
  3242487 (100.00%) were paired; of these:
    2104962 (64.92%) aligned concordantly 0 times
    1113484 (34.34%) aligned concordantly exactly 1 time
    24041 (0.74%) aligned concordantly >1 times
    ----
    2104962 pairs aligned concordantly 0 times; of these:
      222319 (10.56%) aligned discordantly 1 time
    ----
    1882643 pairs aligned 0 times concordantly or discordantly; of these:
      3765286 mates make up the pairs; of these:
        3246063 (86.21%) aligned 0 times
        457695 (12.16%) aligned exactly 1 time
        61528 (1.63%) aligned >1 times
49.94% overall alignment rate
```

Afterwards, we need to obtain the sorted and indexed BAM file from the reads that aligned with the contigs. 

```(bash)
$ samtools view -F 4 -bS Mapping/Sample.sam > Mapping/Sample.bam
$ samtools sort Mapping/Sample.bam > Mapping/Sample.sorted.bam
$ samtools index Mapping/Sample.sorted.bam
$ rm Mapping/Sample.sam Mapping/Sample.bam
```

The first line takes all the reads that aligned to the contigs generated and save it as a BAM file.
* Arguments Defined:
	* `-F 4` : filter the mapped reads
	* `-bS` : output as a BAM file

The second line sorts the BAM file and the third line simply indexes the sorted BAM file. 

The you can remove the large SAM file and unsorted BAM file.

Once you have all the two sorted, indexed BAM files for each population, you can create a contigs database. This database keeps all information related to the contgs: ORFs positions, k-mer frequencies, annotations. This is really useful for visualizing the data with anvi'o (later described). 

```$ anvi-gen-contigs-database -f contigs.fa -o contigs.db -n 'Italian Database'```

Both the contigs dabatase and mapped BAM files will be used in the next two sections of the tutorial.
