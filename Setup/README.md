# Setup and Installation

## SRA Toolkit

The sequencing data for this tutorial is located on the Sequence Read Archive [SRA](https://www.ncbi.nlm.nih.gov/sra) on NCBI. This database stores raw sequence data and alignment information from high-throughput sequencing platforms, allowing for this information to be available for the research community. 

To download the data, the [SRA Toolkit](https://www.ncbi.nlm.nih.gov/sra/docs/toolkitsoft/) must be downloaded and installed.

## Setting up with Bioconda

Downloading and installing bioinformatics programs is easily achieved with Bioconda. Bioconda is a channel for the conda package manager. To read more about Bioconda, click [here](https://bioconda.github.io).

Bioconda requires the conda package manager to be installed.  The best way is to install the Miniconda package (Python 3 version) [here](https://conda.io/miniconda.html).

Once bioconda is installed on your system, the following packages should be installed:
* fastqc
* fastp
* bowtie2
* metaphlan2
* hclust2
* graphlan
* export2graphlan
* anvio
* megahit

To install packages on bioconda, simply type:
`conda install package-name`

## Galaxy

[Galaxy](https://usegalaxy.org) is a web-based platform to perform data analysis.  Some of the programs listed above may be available to perform analysis on the samples.

## Other

Another option is downloading all of these programs locally without BioConda.
* [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* [fastp](https://github.com/OpenGene/fastp)
* [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)
* [metaphlan2](https://bitbucket.org/biobakery/metaphlan2)
* [hclust2](https://bitbucket.org/nsegata/hclust2)
* [graphlan](https://bitbucket.org/nsegata/graphlan/wiki/Home)
* [export2graphlan](https://bitbucket.org/CibioCM/export2graphlan)
* [anvio](http://merenlab.org/2016/06/26/installation-v2/)
* [megahit](https://github.com/voutcn/megahit)

There may be other dependencies that need to be downloaded to run these programs.  Refer to the links provided for further installation instructions.

### References

Grünin Björn, Ryan Dale, Andreas Sjödin, Brad A. Chapman, Jillian Rowe, Christopher H. Tomkins-Tinch, Renan Valieris, the Bioconda Team, and Johannes Köster. 2018. “Bioconda: Sustainable and Comprehensive Software Distribution for the Life Sciences”. Nature Methods, 2018.

