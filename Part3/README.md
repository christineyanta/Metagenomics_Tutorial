# Taxonomical Classification

This part of the tutorial will explore the composition of microbial communities within the human gut microbiome collected the Italian and Hadza populations.

There are many tools that map contigs to classify each one to a taxonomy such as metaBat, maxBin and Concoct.  These methods are typically used for assembly-based metagenomics analysis.

## CONCOCT

CONCOCT is a program that bins metagenomic contigs using nucleotide composition, coverage data and linkage data from paired end reads.

In order to run this program, we must have a contigs.db and a mapping profile for each sample. For this analysis, we can combine the samples for each population and then later compare the CONCOCT results for each population.

From this point, you should have all BAM files for each sample in each population and contigs.db created.

The next step is to create a profile for anvi'o.  This can be done with the following command:

```$ anvi-profile -i sample.bam -c contigs.db```

This will create a PROFILE.db file.  This step should be done for all .bam files created in the previous section.

Afterwards, you will want to merge all sample for each population:

```$ anvi-merge */PROFILE.db -o SAMPLES-MERGED -c contigs.db```

In the end, you should have two SAMPLES-MERGED directories, one for each population (Hadza and Italian).

Finally, to run CONCOCT for each population:

```$ anvi-summarize -p SAMPLES-MERGED/PROFILE.db -c contigs.db -o SAMPLES-SUMMARY -C CONCOCT```

From here, two SAMPLES-SUMMARY directories will be made with the results from CONCOCT. This data can be further analyzed. For instance, for each bin, you can examine the contigs, gene calls, abundances, and much more! 


**Note: With the merged_profile made of the samples, you can use anvi'o interactive interface to browse the data and perform refined, supervised binning.  For more information, click [here](http://merenlab.org/2016/02/27/the-anvio-interactive-interface/).

```anvi-interactive -p SAMPLES-MERGED/PROFILE.db -c contigs.db -C CONCOCT```

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

To get species only abundance table:

```$ grep -E "(s__)|(^ID)" merged_tax_table.txt | grep -v "t__" | sed 's/^.*s__//g' > merged_tax_species.txt```

### Visualizing MetaPhlAn2 Results

The merged taxonomic abundance results can then be visualized with the [hclust2](https://bitbucket.org/nsegata/hclust2) program. This tool is useful for displaying the data as a heat map.

To generate the heatmap, simply type:

```$ hclust2.py -i merged_tax_species.txt -o abundance_heatmap_species.png --ftop 25 --f_dist_f braycurtis --s_dist_f braycurtis --cell_aspect_ratio 0.5 -l --flabel_size 6 --slabel_size 6 --max_flabel_len 100 --max_slabel_len 100 --minv 0.1 --dpi 300```

The arguments are as follows:
* `-i merged_tax_species.txt` : input species abundance table file
* `-o abundance_heatmap_species.png` : output file for heatmap
* `--ftop 25` : select the top 25 species
* `--f_dist_f braycurtis` : use Bray-Curtis as the distance measure between microbes
* `--s_dist_f braycurtis` : use Bray-Curtis as the distance measure between samples
* `--cell_aspect_ratio 0.5`: set the cell aspect ratio of 0.5
* `-l` : use a log scale for assigning colours
* `--flabel_size 6` : set the feature label size to 6
* `--slabel_size 6` : set the sample label size to 6
* `--max_flabel_len 100` : max label length of 100
* `--max_slabel_len 100` : max label length of 100
* `--minv 0.1` : minimum display value of 0.1
* `--dpi 300` : set image resolution to 300

The heatmap will then saved under the directory you are working in.  Take a look at it to observe the taxonomical differences between samples.

### Visualizing MetaPhlAn2 Results with GraPhlAn

GraPhlAn allows the species abundance results in each sample to be visualized as a phylogeny.  The [GraPhlAn](http://huttenhower.sph.harvard.edu/graphlan) tool provides as easy to create trees and annotate them with microbial species names and relative abundances.

To create a GraPhlAn visualization, we first need to create an appropriate input file. For the merged_tax_table.txt generated above, perform the following command:

```$ export2graphlan.py --skip_rows 1,2 -i merged_tax_table.txt --tree merged_abundance.tree.txt --annotation merged_abundance.annot.txt --most_abundant 100  --annotations 5,6 --external_annotations 7```

The arguments are as follows:
* `--skip_rows 1,2` : skip rows 1 and 2
* `-i merged_tax_table.txt` : input abundance table
* `--tree merged_abundance.tree.txt` : output file used to build tree
* `--annotation merged_abundance.annot.txt` : output file used for annotation
* `--most_abundant 100` : select top 100 species that are most abundant
* `--annotations 5,6` : select taxonomic levels 5 and 6 to annotate
* `--external_annotations 7` : use taxonomic level 7 for external legen

This command will output two files: *.tree.txt and *.annot.txt. These two files are then used to create the cladogram using the following command:

```(bash)
$ graphlan_annotate.py --annot merged_abundance.annot.txt merged_abundance.tree.txt merged_abundance.xml
$ graphlan.py --dpi 300 merged_abundance.xml merged_abundance.png --external_legends
```

The cladogram `.png` file is now generated and can be found in the directory you are working with.
