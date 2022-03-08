# Team2-GenePrediction

The Genome Prediction group members for Team 2 are:
* Mahale Aishwarya
* Rachel Calder
* Ravindra Raju Dinesh
* Gabe DuBose
* Howard Page
* Bhattaram Swethasree
* Zheying Xu


## Installation

```
## Prokka Installation
conda install -c conda-forge -c bioconda prokka
prokka --setupdb
```
## Prediction with Prokka
The command used was
```
prokka <assembly.fasta> --outdir <output/path/> --metagenome --kingdom Bacteria --rfam
```
