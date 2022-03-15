# Team3-GenePrediction  
  
## Introduction  
Gene Prediction using Prodigal and GeneMark to predict protein coding genes and using Prokka to predict non coding RNA genes.  
  
## Software Dependencies  
* Prodigal  
* GeneMarkS-2  
* Prokka  
* MergeGFF  
* ORForise  
  
## Pipeline Summary  
### Predicting Protein Coding Genes  
1. Prodigal  
2. GeneMark  
  
### Predicting Non-Coding RNA Genes  
1. Prokka  
  
### Merging GFF files  
1. MergeGFF  

    1. gffcompare  
    2. gffread
  
## Usage  

### Prerequisites-  
1. Install dependencies listed above.  
2. Create a directory containing fasta files.  
3. Make an environment from Prokka, install prokka with conda  


### Running gene prediction for Non-Coding
```
usage: gp-pipe-nc.py [-h] -i I -o O

Arguments:
  -h, --help  show this help message and exit
  -i I        Input folder path containing contigs
  -o O        Output folder path
```
### Running gene prediction for Coding
```
usage: gp-pipe-c.py [-h] -i I -o O

Arguments:
  -h, --help  show this help message and exit
  -i I        Input folder path containing contigs
  -o O        Output folder path
```
  
  
### Ab Initio Tools  
#### Prodigal  
    
    prodigal -i [path to input fasta file] -d [Output FASTA file path] -f gff -o [Output gff path]

#### GeneMarkS-2  
    
    ./gene_mark_s.py -i <input> -o <output>
 
  
#### Prokka  
    
    prokka --outdir {name of directory} --prefix {name of contig} --quiet contigs.fa
  
  
## Members  
|||||||||
|--|--|--|--|--|--|--|--|
|SAIDEEP NARENDRULA| COLIN NAUGHTON | ASHIKA RAMESH |DHRUV KARVE |PIYUS MOHANTY| SIMING ZHAO| CHENG ZHANG |NIDHI KOUNDINYA|  
