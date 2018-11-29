# Setup and Installation

## SRA Toolkit

The sequencing data for this tutorial is located on the Sequence Read Archive [SRA](https://www.ncbi.nlm.nih.gov/sra) on NCBI. This database stores raw sequence data and alignment information from high-throughput sequencing platforms, allowing for this information to be available for the research community. 

To download the data, the [SRA Toolkit](https://www.ncbi.nlm.nih.gov/sra/docs/toolkitsoft/) must be downloaded and installed.

## Setting up with Bioconda

Downloading and installing bioinformatics programs is easily achieved with Bioconda. Bioconda is a channel for the conda package manager. To read more about Bioconda, click [here](https://bioconda.github.io).

Bioconda requires the conda package manager to be installed.  The best way is to install the Miniconda package (Python 3 version) [here](https://conda.io/miniconda.html).

Once bioconda is installed on your system, the following packages should be installed:
* fastp
* bowtie2
* metaphlan2
* anvio
* megahit

To install packages on bioconda, simply type:
'conda install <package-name>'

### References

Grünin Björn, Ryan Dale, Andreas Sjödin, Brad A. Chapman, Jillian Rowe, Christopher H. Tomkins-Tinch, Renan Valieris, the Bioconda Team, and Johannes Köster. 2018. “Bioconda: Sustainable and Comprehensive Software Distribution for the Life Sciences”. Nature Methods, 2018.

