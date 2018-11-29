## Analyzing Shotgun Metagenomic Data

# Introduction
The purpose of this tutorial is to provide one of the possible metagenomic workflows for analyzing shotgun metagenomic next-generation sequencing reads. Keep in mind that there are many different programs that will accomplish each step.

Metagenomics is the study of all genetic material within an environmental sample. Assembly-based metagenomics attempts to assemble the reads from the sample(s) to create contigs and 'bin' each contig into genomes.  This will later allow taxonomical and functional profiling.

This tutorial will perform a comparative genomics study on the human gut microbiome of two populations: Hadza hunter-gatherers in Africa and Western urban Italian adults. This study was performed by Rampelli et al. and all data used in this tutorial was obtained from this study.
* Initial Study: [Schnorr et al., 2014](https://christineyanta.github.io/Metagenomics_Tutorial/Schorr2014_HadzaGutMicrobiome.pdf)
* Further Study: [Rampelli et al., 2015](https://christineyanta.github.io/Metagenomics_Tutorial/Rampelli2015_HadzaGutMicrobiota.pdf)

To summarize, these two populations differ in their diets. The Hadza diet consists of wild foods, such as meat, honey and berries, that have not undergone cultivation or domestication.  Alternatively, the Italian diet consists entirely of commercial agricultural products that primarily includes plants foods and pasta, along with some dairy and poultry.  

Overall, this tutorial will examine a subset of this data to ultimately determine the taxnomical differences between both populations in addition to the functional difference.

# Important Note

This tutorial was created for the Final Independent Study of the Genomics (BIOL 469) course offered at the University of Waterloo (Fall 2018). 

# Requirements and Software Installation

The main requirements to follow along with tutorial is to have access to a Linux-based OS. 

There are a variety of programs that are required to be downloaded in order to analyze the data set.  Please refer to the [Setup Instructions](https://christineyanta.github.io/Metagenomics_Tutorial/Setup/) before continuing with this tutorial.
  
# Metagenomics Analysis

A metagenomic study starts much earlier than sequence analysis. The following steps must be considered and performed: experimental design, sampling strategies, library preparation and sequencing. These four steps are critical to obtain good-quality data to analyze.

This tutorial begins once the sequencing reads for each sample have been obtained. It is divided into ...  continous sections.

1. [Obtaining and Filtering Data](https://christineyanta.github.io/Metagenomics_Tutorial/Part1)
2. [Read Assembly and Mapping](https://christineyanta.github.io/Metagenomics_Tutorial/Part2)
3. [Taxonomical Classification](https://christineyanta.github.io/Metagenomics_Tutorial/Part3)
4. [Functional Classification](https://christineyanta.github.io/Metagenomics_Tutorial/Part4)
