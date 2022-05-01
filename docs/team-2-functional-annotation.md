# Team2-FunctionalAnnotation

Gene prediction fasta files from genome assemblies are functionally annotated to aid in the identification of biological significance. Genes, transmembrane helices signal peptide and virulence factors are identified through functionalannotation.py. Operon detection was considered. Due to tool deprecation and run-time concerns for alternative methods, operon annotation was left out of the pipeline, but the code can be viewed at operons.py. Please note the installation requirements in the overview below.

![alt text](https://github.gatech.edu/computationalgenomics2022/Team2-FunctionalAnnotation/blob/main/FinalPipeline.png)


## USEARCH 

Using USEARCH to remove the redundancy in the genome, greatly reduced the running time of the prediction. The clustering using USEARCH generates two output files, one is used for the following prediction (functional annotation), and the other is used to give the functional annotation results to these genes which are removed in the clustering step.  

1. Combine nucleotide sequences

         cat *.fnn > <out.fna>

2. Combine amino acid sequences

         cat *.faa > <out.faa>

3. Temporarily add USEARCH to PATH */ 

export PATH=$PATH:/home/groupb/bin/ 

4. Run USEARCH

         usearch11.0.667_i86linux32 -cluster_fast combined.fna -id 0.9 -centroids combined.clustered.fna -uc clusters.fna.uc 

         usearch11.0.667_i86linux32 -cluster_fast combined.faa -id 0.9 -centroids combined.clustered.faa -uc clusters.faa.uc 

 


Expected generated files: 

    clusters.faa.uc 

    clusters.fna.uc 

    combined.clustered.faa 

    combined.clustered.fna 

    combined.faa 

    combined.fna 




## SignalP 

@description 

SignalP is a tool based on the ab initio method to find out the signal peptides on the given amino acid sequence. For the 5th version, you should state which model you decide to use: eukarya, archaea, Gram-positive bacteria and Gram-negative bacteria. While for the 6th version, you can just clarify whether the prediction target is eukarya or not. Finally, SignalP will give out the probability of containing different type of signal peptides or others, for each gene. 

 
1. Add signalp to PATH
 
2. Change directory (or you can modify the relative path of fasta file)

4. Run command

         signalp -fasta combined.clustered.faa -org gram- -gff3 -format short 

 

The expected two generated files: 

         combined.clustered.gff3 

         combined.clustered_summary.signalp5 


## PHOBIUS 

Phobius is a tool that is based on a Hidden Markov Model capable of predicting both Transmembrane Helices and Signal peptides.  

Prerequisites- Unix/Linux, Perl 5.6 or later  

 The program takes proteins in FASTA format. It recognizes the 20 amino acids and B, Z, and X, which are all treated equally as unknown. 

There are two output formats: Long and short. We used the short format. In the short output format one line is produced for each protein with no graphics. Each line starts with the sequence identifier and then these fields: 

1. "TM": The number of predicted transmembrane segments. 

2. "SP": Y/N indicator if a signal peptide was predicted or not. 

3. "PREDICTION": Predicted topology of the protein. The topology is  given as the 	position of the transmembrane helices separated by 'i' if the 	loop is on the cytoplasmic or 	'o' if it is on the non-cytoplasmic side. 
 


         phobius.pl -short <input faa file> > <output text file> 

The output text file is then converted to a gff file using a separate script. The output is filtered such that the gff file contains only the transmembrane helices annotation. This was done to reduce redundancy between PHOBIUS and SignalP annotations.  

Run time- Around 4 minutes for the clustered fasta file 

 

## EggNOG – mapper v2.1.7 

EggNOG - mapper is a tool for functional annotation of large sets of sequences. Infers fast orthology calls based on precomputed clusters and phylogenies from eggNOG database. The database contains Orthologous Groups of proteins at taxonomic levels. EggNOG has two modes: hmm and diamond. We used the diamond method to search for the best seed orthologs located in eggNOG protein database since it is fast. 

Input: Fasta file from clustering  

Output: output.annotation, output.hits, output.seed_orthologs  

Commands used:  

1. Download the bacteria database   

         python2.7 download_eggnog_data.py bact   

      

2. Mapping using diamond method   

         /../emapper.py -i input_file -o output -d bact -m diamond   
 

Output: output.annotation, output.hits, output.seed_orthologs     

Run-time: ~70 min 

There were a total of 6624 (out of 11178 sequences) annotations that were predicted. The ontologies from databases are GO terms, KEGG, PFAM  

 

## VFDB – Virulence Factor Database 

VFDB is a database of virulence factors which search for factors that contribute to pathogenecity within the sample file. The VFDB is paired with Blast. There is the option to use the core dataset or the full dataset. The core dataset only contains lab-tested virulence factors. We used the full dataset which in more inclusive and provides more options for virulence factor detection. 

Input: Fasta file from clustering, VFDB 

Preparation:  

         makeblastdb –infile <VFDB Download> 

Usage: 


         blastp -db VFDB_setB_pro.fas -query <fasta> -num_threads 4 -evalue 1e-10 -outfmt 6 –out <blast out> 

Conversion to gff: 

Install blast2gff: https://github.com/guigolab/blast2gff 

         blast2gff blastdb –db <your db> -a <your fasta> <blast out> <gff file name> 
         

## .gff combination 
This portion of the script aims to simply combine functional annotation results. The script mainly performs two things: combination and removing the repeats. For those genes with functional annotation results given by multiple tools, remain the first encountered. 


 # Functional Annotation Results
 
 
 ### Phobius
 
There were a total of 1535 (out of 11178 sequences) transmembrane proteins that were predicted. Total number of transmembrane helices predicted for the clustered input file- 4809 
 
 
 ### SignalP
 570 Signal Peptides were found via SignalP in the clustered protein sequences. There are 368 SP(Sec/SPI), 49 TAT(Tat/SPI) and 153 LIPO(SecSPII), respectively.

 ### VFDB
 49022 virulence factor hits were found (not species specific).
 
 ### Eggnog Mapper
 6624 orthologs were identified
 
 ### Final Combined GFF Results
 7157 total unique annotations


