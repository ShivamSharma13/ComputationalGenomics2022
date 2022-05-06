# Team3-GenePrediction  
  
## Introduction  
Gene Prediction using *Prodigal* and *GeneMark* to predict protein coding genes and using *Prokka* to predict non coding RNA genes.  
Input - *Isolates from genome assembly*
  
## Software Dependencies  
* Prodigal 
* GeneMarkS
* Prokka
* MergeGFF  
* ORForise  
  
## Pipeline Summary  
  
![Final Pipeline](https://github.gatech.edu/computationalgenomics2022/Team3-GenePrediction/blob/main/files/final_results/final_pipeline.png)
  
  
### Predicting Protein Coding Genes  
1. **Prodigal** - PROkaryotic DYnamic programming Gene-finding ALgorithm, a fast, lightweight, open source, Ab Initio gene prediction program with less false positive.
2. **GeneMark** - Identifies open reading frames containing real genes, uses HMM models and Gibbs sampling mutliple alignment program
  
### Predicting Non-Coding RNA Genes  
1. **Prokka** - An ensemble of software tools that has shown to produce good results for gene prediction of  genomic bacterial sequences.
           
           Includes:-
           1) Prodigal- Predicts coding sequences
           2) RNAmmer- Predicts ribosomal-RNA genes in genome from DNA FASTA sequences​
           3) SignalP- Predicts the presence of signal peptides and the location of their cleavage sites
           4) Infernal- INFERence of RNA ALignment
  
### Merging GFF files  
1. **MergeGFF**  

    * *gffcompare* - used to compare, merge, annotate and estimate accuracy of one or more GFF files 
    * *gffread* - utility providing format conversions, filtering, FASTA sequence extraction
  
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
  
## Final Results  
### ORForise Results  
  
![ORForise plot](https://github.gatech.edu/computationalgenomics2022/Team3-GenePrediction/blob/main/files/final_results/orforise_results.png)
  
  
Final output files 
-----
*Coding Regions*  

**GeneMarkS-2 output:** ```/home/groupc/analysis/Team3-GenePrediction/genemarks_out/```  
  
**Prodigal output:** ```/home/groupc/analysis/Team3-GenePrediction/prodigal_output_tool/```  
  
**Combined gff files (Prodigal + GMS2):** ```/home/groupc/analysis/Team3-GenePrediction/combined_gff/```  

-----
*Non-Coding Regions*  

**Prokka output:** ```/home/groupc/files/gene_prediction/prokka/output/```
  
## Members  
|||||||||
|--|--|--|--|--|--|--|--|
|SAIDEEP NARENDRULA| COLIN NAUGHTON | ASHIKA RAMESH |DHRUV KARVE |PIYUS MOHANTY| SIMING ZHAO| CHENG ZHANG |NIDHI KOUNDINYA|  
