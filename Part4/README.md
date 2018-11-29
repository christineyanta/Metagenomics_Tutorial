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


 
