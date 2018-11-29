# Functional Classification

At this point, you should have created a contigs.db file for each population.  If not, please go back to [Read Assembly and Mapping](https://christineyanta.github.io/Metagenomics_Tutorial/Part2) before continuing.

This section will focus on obtaining gene information from the contigs gathered from each population. We will focus on getting this information from the tool: [GhostKOALA](https://www.kegg.jp/ghostkoala/).

GhostKOALA (KEGG Orthology And Links Annotation) is an annotation tool that assigns KEGG Identifiers to the assembled metagenome. With this data, we can compare functional profiles between the two populations under study.

## Getting Started

To begin, we need to obtain all gene calls from the contigs database built for each population. This can be done as follows:

```$ anvi-get-sequences-for-gene-calls -c Contigs/contigs.db --get-aa-sequences -o protein-sequences.fa```

The arguments are as follows:
* `-c Contigs/contigs.db` : path to contigs database
* `--get-aa-sequences` : get all gene calls from the contigs database and translate to protein sequences
* `-o protein-sequences.fa` : output fasta file containing amino acid sequences of possible genes

Note: This is a standard code snippet. You will need to perform this command for both Italian and Hadza populations using the appropriate contig databases.

## GhostKOALA

Once the amino acid sequence fasta files are obtained, you can input the files (one at a time) to the [GhostKOALA webserver](https://www.kegg.jp/ghostkoala/).

1. Upload the file by clicking 'Choose File'
2. Choose Appropriate KEGG Genes Database (genus_prokaryotes + family_eukaryotes)
3. Enter your e-mail address
4. Accept the request on your e-mail to start the job.

Once the analysis is done, you will receive an e-mail with the results.  From there, download the functional results.

## Analyzing Results from GhostKOALA

In order to convert the KEGG IDs to their function.  Therefore, you will need to download the following file that links the KEGG assignment IDs to their functions:

```wget https://christineyanta.github.io/Metagenomics_Tutorial/Part4/KEGG_table.txt```

Next, we will need to perform some text file manipulations to get the data that we want. For this example, we will use the GhostKOALA user_ko.txt file obtained from the Italian data set.

At this point, we should have an annotation file that appears similar to this:

```(bash)
$ head user_ko.txt
genecall_0	K01785
genecall_1	K07240
genecall_2	K02049
genecall_3	K02050
genecall_4
genecall_5
genecall_6	K02493
```

For relative gene abundance analysis, all we need is the KEGG IDs.  Therefore, we can extract them as follows:

```$ awk '{print $2}' italian_ko.txt > italian_ko_keggids.txt```

Now we want to map the KEGG IDs to their function.  We can do this by:

```$ awk 'NR==FNR{a[$1]++;next};a[$1]' italian_ko_keggids.txt KEGG_table.txt > Italian_functions.txt```

This will now give us the functions and pathways associated for each gene that was found. From here, we can use this data to determine relative gene abundances.

For instance, say we want to get a count on number of genes to overall pathways. We can first get the second column from the functions file and then count how many there are of each:

```(bash)
$ awk '{print $2}' Italian_functions.txt > Italian_pathways.txt
$ cat Italian_pathways.txt | sort | uniq -c > Italian_pathways_count.txt
```

If we look at this file, we now see the number of genes that were present for each major pathway:

```(bash)
$ cat Italian_pathways_count.txt
 182 Cellular Processes
 322 Environmental Information Processing
 230 Genetic Information Processing
 176 Human Diseases
1321 Metabolism
 373 Unknown
 128 Organismal
```

This data can be obtained from both Italian and Hadza populations in similar fashions. Once the data is obtained, relative abundance profiles can be created and analyzed.

For further results, please refer to Final Project Report.

