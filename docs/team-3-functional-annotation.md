# Team3-FunctionalAnnotation

## Introduction
Listeria monocytogenes is a food-borne pathogen that primarily afflicts immunocompromised individuals and can provoke septicemia, meningitis and fetal infection or abortion in infected pregnant women. In the last few years, several reported outbreaks and sporadic cases associated with consumption of contaminated meat and meat products with L. monocytogenes have increased in developing countries and USA. A latest one listed on FDA for Dole Salad. Here we propose a pipeline for the Functional annotation to predict virulence factors and pathogenicity.
## Software Dependencies
### Installation using conda
create a conda env 
```
conda create --name team3_functional_annotation aragorn barrnap blast diamond eggnog-mapper hmmer infernal prodigal prokka torch 
```
* usearch11.0.667_i86linux32
* signalp 6.0
* blastp
    * Create virulance factor database with
```
makeblastdb -in VFDB_setB_pro.fas -dbtype pro
```
* deeparg
## Pipeline Summary

![Visual of the pipeline](Pipeline.png?raw=true "Pipeline")

We first cluster the genes to reduce the runtime for all other operations.

Next, we use signalp to identify signal peptides, phobius to identify transmembrane proteins, blasp + vsdb to identify virulance related genes, and deeparg to identify antibiotic resistance genes. We next merge the resultant gff files through concatination.
## Usage
### Uclust
#### FASTA Nucleic acid
first, create a combined nucleic acid fasta file (.fna) for all 50 isolates
```
cat /home/groupc/analysis/Team3-GenePrediction/combined_gff/*.fa >/home/groupc/files/functional_annotation/combined_isolates/combined_all_isolates.fa
```
then run uclust using
```
usearch11.0.667_i86linux32 -id 0.97 -cluster_fast /home/groupc/files/functional_annotation/combined_isolates/combined_all_isolates.fa -centroids /home/groupc/files/functional_annotation/uclust/nr_097.faa -uc /home/groupc/files/functional_annotation/uclust/hpt/nr_097.faa.uc -sort length
```
#### FASTA Amino acid
first, create a combined amino acid fasta file (.faa) for all 50 isolates
```
cat /home/groupc/files/gene_prediction/prokka/output/CGT1*/*.faa >/home/groupc/files/functional_annotation/combined_isolates/combined_prokka.faa
```
then run uclust using
```
usearch11.0.667_i86linux32 -id 0.97 -cluster_fast /home/groupc/files/functional_annotation/combined_isolates/combined_prokka.faa -centroids /home/groupc/files/functional_annotation/uclust/hpt/nr_097.faa -uc /home/groupc/files/functional_annotation/uclust/hpt/nr_097.faa.uc -sort length
```
### SignalP 6.0

```
signalp6 -ff /home/groupc/files/functional_annotation/uclust/nr3.faa -fmt none -od /home/groupc/files/functionnal_annotaion/signalp
```
### Phobius
```
/home/groupc/tmp/tmpckJtGA/phobius/phobius.pl -short nr3.faa > nr3.txt
```
### Blastp VFDB
```
blastp -db VFDB_setB_pro.fas -query /home/groupc/files/functional_annotation/uclust/nr3.faa -out output.out -max_target_seqs 1 -max_hsps 1 -outfmt "6 qseqid qstart qend sseqid evalue sstart send sframe stitle" -evalue 1e-10
```
### DeepArg
```
deeparg predict --model SS --type nucl -i "/home/groupc/files/functional_annotation/uclust/nr2.fasta" -o "/home/groupc/files/functionnal_annotaion/deeparg_output/deeparg_output.out" -d /home/groupc/bin/deeparg_downloads/
```
