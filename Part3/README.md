# Taxonomical Classification

This part of the tutorial will explore the composition of microbial communities within the human gut microbiome collected the Italian and Hadza populations.

There are many tools that map contigs to classify each one to a taxonomy such as metaBat, maxBin and Concoct.  These methods are typically used for assembly-based metagenomics analysis.

## Concoct

## MetaPhlAn2

Alternatively, a read-based approach can also be used to classify the reads taxonomically.  The tool which we will go through is MetaPhlAn2.

[MetaPhlAn2](https://bitbucket.org/biobakery/metaphlan2) is a popular tool used to profile microbial communities from metagenomic data (not marker specific). It uses a k-mer based approach to classify the sequencing reads rapidly and accurately.

This workflow begins by running MetaPhlAn2 on each sample and then merging the results for each population. You can run this tool by using the following command:

```$ metaphlan2 Sample.R1.fq --input_type fastq --nproc 4 > sample_tax_profile.txt```

The arguments are as follows:
* `--input_type` : metaphlan2 can accept fasta, fastq, fasta.gz, fastq.gz as input files
* `--nproc 4`: run the analysis using 4 cores

If you have multiple samples within the same directory, the command can be simplified to:

```(bash)
$ for f in *.fq
> do
>	metaphlan2 $f --input_type fastq --nproc 4 > ${f%.fq}_tax_profile.txt
> done
```

Either way, this analysis will produce a table in the output file with the relative abundance of taxonomy found.

```(base)
$ less sample_tax_profile.txt
#SampleID       Metaphlan2_Analysis
k__Bacteria     99.893
k__Viruses      0.107
k__Bacteria|p__Firmicutes       90.50526
k__Bacteria|p__Actinobacteria   7.13968
k__Bacteria|p__Bacteroidetes    2.24805
k__Viruses|p__Viruses_noname    0.107
k__Bacteria|p__Firmicutes|c__Clostridia 88.5198
k__Bacteria|p__Actinobacteria|c__Actinobacteria 7.13968
k__Bacteria|p__Bacteroidetes|c__Bacteroidia     2.24805
k__Bacteria|p__Firmicutes|c__Negativicutes      1.49463
k__Bacteria|p__Firmicutes|c__Erysipelotrichia   0.49084
k__Viruses|p__Viruses_noname|c__Viruses_noname  0.107
```

Once the taxonomy information is gathered for all samples within one population, it can't be merged with the following command:

```$ merge_metaphlan_tables.py *_tax_profile.txt > merged_tax.txt```

Taking a look at the output file, you can notice that all results for each sample within the population has been merged.

### Visualizing MetaPhlAn2 Results

The merged taxonomic abundance results can then be visualized the the [hclust2](https://bitbucket.org/nsegata/hclust2) program. This tool is useful for displaying the data as a heat map.

  
