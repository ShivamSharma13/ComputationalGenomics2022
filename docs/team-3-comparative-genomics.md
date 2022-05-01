
# Team 3 - Comparative Genomics

This pipeline aims to compare *Listeria monocytogenes* isolates in order to identify an outbreak, find food groups associated with the outbreak, and find antibiotic resistance or virulence genes in the outbreak genomes.

**Authors:** Kirti Deepak Chhatlani, Katherine Duchesneau, Xu Qiu, Ashika Ramesh, Huy Tran, Joseph Luper Tsenum, Cheng Zhang
## Comparative Genomics Pipeline

This pipeline aims to compare *Listeria monocytogenes* isolates in order to identify an outbreak, find food groups associated with the outbreak, and find antibiotic resistance or virulence genes in the outbreak genomes.

The tools are divided into four categories as follows:

* Gene specific phylogenies with ```stringmlst``` and ```chewbbaca```
* Single nucleotide polymorphism phylogeny with ```kSNP3```
* Whole genome phylogeny with ```fastANI```
* Annotations with ```abricate```



The first three categories of tools build phylogenetic trees from the *L. monocytogenes* samples using different methods that allow us to visualize and identify regions of rapid expansion associated with outbreaks. The fourth category includes tools to functionally annotates the *L. monocytogenes* samples and identify genes related to virulance or antimicrobial resistance that will inform treatment.

You can run the scripts as follows:
```comparative_genomic.py -i <directory_contigs> -o <kSNP_outputdir>```
## 1. Gene-by-gene Analysis Using stringMLST and chewBBACA
We used stringMLST to check the sequecne type (ST) of our isolates while chewBBACA was used for extracting core genome multilocus sequence typing (cgMLST) from our isolates. We created phylogenetic trees to visualize our outputs from stringMLST and chewBBACA.

### 1.1. stringMLST

sringMLST is a fast k-mer based tool for matching allele sequences of 7 housekeeping genes of isolates. We predicted the sequence type of our *L. monocytogene* isolates using PubMLST database.

#### 1.1.1. Installation

For basic stringMLST usage to build database and predict sequence types, no Python dependencies are required.

```conda install -c bioconda stringmlst```

#### 1.1.2. Usage

stringMLST takes raw reads as input. We used reads before and after FastP from genome assembly for our ST detection and both gave us the same STs as our output. We therefore decided to go for FastP files as our input data. We downloaded *L. monocytogenes* database from PubMLST for our stringMLST analysis.

stringMLST.py --getMLST -P listeria/lsa --species Listeria. The downloaded *L. monocytogenes* database came with lsa_35.txt, lsa_weight.txt and lsa_profile.txt files, so there was no need to build the database, nevertheless we built the database but got the same outputs as the ones we downloaded.

```stringMLST.py --buildDB --config lsa_config.txt -k 35 -P lsa```

For ST discovery of our *L. monocytogenes*, we used batch mode by default to process all the 50 isolates at once.

```stringMLST.py --predict -d <directory for samples> -p --prefix <prefix for the database> -k <k-mer size> -o <output file name>```

In our case, we used:

```stringMLST.py --predict -d QCdata -P listeria/lsa -k 35 -o StringMLST_Listeria```

#### 1.1.3. Outputs

Our stringMLST output file comprised of the 7 housekeeping genes and query genomes of our *L. monocytogenes* isolates. We converted this output file into a Newick format using Grapetree.

```grapetree --profile StringMLST_Listeria --method MSTreeV2 > output.tree```

Using Plottree's ETE2 web server, we created a phylogenetic tree for our stringMLST taking stringMLST.nwk as our input (visit http://etetoolkit.org/treeview/).
Alternatively, we installed plottree by pip install plottree and used the command below to create the phylogenetic tree

* ```-l```: represents height and can be reset to improve tree visualization

```plottree stringMLST.nwk -l 8.4 -o TreestringMLST```

### 1.2. chewBBACA

ChewBBACA is a BSR-based allele calling tool for gene-by-gene MLST analysis. ChewBBACA is a comprehensive and highly efficient software for determining allelic profiles and validating wg/cgMLST schemas. Allele calling is achieved by BLAST Score Ratio (BSR). It takes assemblies (contigs) as input. Steps for chewBBACA analysis include wgMLST/cgMLST schema creation, allele calling and schema evaluation.

#### 1.2.1. Installation

The main Python dependencies for chewBBACA's wgMLST/cgMLST schema creation, allele calling and schema evaluation include BLAST 2.9.0+, Prodigal 2.6.0 and MAFFT. We installed this software through conda since it takes care of these dependencies with their required libraries.

```conda install -c bioconda prodigal```

#### 1.2.2. Usage

Since https://chewbbaca.online/stats contains the schema of our *L. monocytogenes* isolates, we didn't create wgMLST/cgMLST schemas. We downloaded chewBBACA's schema for our isolate through the command below:

```chewBBACA.py DownloadSchema -sp 6 -sc 1 -o Listeria```

Next, we performed allele calling with the downloaded wgMLST schema (the value for --cpu can be reset). This generated logging_info.txt, RepeatedLoci.txt, results_alleles.tsv, results_contigsInfo.tsv, and results_statistics.tsv files.

* ```--cpu```: parameter to enable parallelization and reduce run time

```chewBBACA.py AlleleCall -i data -g Listeria_db -o Allele --cpu 4 --ptf Listeria_db/Listeria_monocytogenes.trn```

We then tested our genome quality for cgMLST to remove repeated reads. The value for -n, -t, and -s can be reset as well). This generated Genes_95%.txt, GenomeQualityPlot.html, and removedGenomes.txt files.

```chewBBACA.py TestGenomeQuality -i Alleleresults/results_alleles.tsv -n 1 -t 100 -s 5 -o Test``` 

Lastly, we extracted only the cgMLST loci present in 95% of the matrix, This generated cgMLSTschema.txt, cgMLST.tsv, mdata_stats.tsv and Presence_Absence.tsv files as our output.

```chewBBACA.py ExtractCgMLST -i Alleleresults/results_alleles.tsv --r Alleleresults/RepeatedLoci.txt --g Test/removedGenomes.txt -o Extract --t 0.95```

#### 1.2.3. Outputs

Our final output for chewBBACA analysis was a cgMLST.tsv file containing the core genome multilocus sequence typing of our *L. monocytogenes*. To create our phylogenetic tree for chewBBACA's cgMLST, we first converted the cgMLST.tsv file into a Newick file using Grapetree as shown below:

```grapetree --profile Extract/cgMLST.tsv --method NJ > chewbbaca.nwk```

Again, we used Plottree's ETE2 web server to create a phylogenetic tree for our chewBBACA's cgMLST taking chewbbaca.nwk as our input (visit http://etetoolkit.org/treeview/).

Alternatively, we installed plottree by pip install plottree and used the command below to create the phylogenetic tree.

* ```-l```: represents height and can be reset to improve tree visualization

```plottree chewbbaca.nwk -l 8.4 -o chewbbaca```

## 2. SNP tree building with kSNP3
kSNP3 identifies the pan-genome single nucleotide polymorphism in a set of genomes without the need for a reference genome. It outputes a phylip matrix and phylogenetic trees (newick format)

### 2.1. Installation
kSNP3 requires tcsh, which can be installed through conda.

```conda install -c conda-forge tcsh```

Once tcsh is installed, get kSNP3 and add it to your path.

```
wget https://sourceforge.net/projects/ksnp/files/kSNP3.021_Linux_package.zip # Download the program
unzip kSNP3.021_Linux_package.zip
export PATH=$PATH:$HOME/../groupc/bin/kSNP3.021_Linux_package/kSNP3 # Add it to your path
```

Manual installation of kSNP3 requires some modification of the files. In the directory you downloaded above, you can find directory kSNP3/ which contains file kSNP3. In this file, change the shebang to point to the location of tcsh (you can find it using ```which tcsh```)

```
nano /home/groupc/bin/kSNP3.021_Linux_package/kSNP3/kSNP3
# At the top, change the #! from #!/bin/tcsh to the location of the tcsh
# for example, we changed it to /home/groupc/anaconda3/envs/team3_comparative_genomics/bin/tcsh
```

Then, comment line 7 to deactivate it and add the location of this folder to line 8. These lines should now look as follows:

```
# Set kSNP=/usr/local/kSNP3
set kSNP=/home/groupc/bin/kSNP3.021_Linux_package/kSNP3/
```

Test kSNP3 by typing ```kSNP3```. The help documentation should print if kSNP3 is installed correctly.

### 2.2. Usage

kSNP3 can take raw reads or genomes as input. This example uses assembled genomes (aka contigs).

The kSNP command, however, does not use the directory as input but rather a tab separated textfile with two columns, the first being the location of each genome on your machine and the second the genome name. The script attached to this repository contains code to produce this file. kSNP3 also requires the user to input a k value, which can be estimated using the ```Kchooser``` command. The script in this repository automatically runs ```Kchooser``` and uses the output value as its k.

```
MakeFasta in_list.txt kchooser.fasta # Make a single fasta file to input into kchooser 
Kchooser kchooser.fasta # Find the best k value for your data
kSNP3 -in in_list.txt -k <int> -outdir <output directory> -ML -NJ
```

### 2.3. Outputs

kSNP3 outputs a phylip matrix and newick formated trees. The default is to use the parsimony tree building method but flags ```-NJ -ML``` add maximum likelihood and neighborjoing trees to the output.

The final visualization can be obtained by feeding the newick formated trees in R using ```ggtree```.

## 3. ANI tree building with FastANI
Developed for fast alignment-free computation of whole-genome Average Nucleotide Identity (ANI).

### 3.1. Installation
FASTANI can be installed through conda.
```conda install -c bioconda fastani```

### 3.2. Usage
```fastani --rl <reference genome> --ql <query genome> -o fastani.out```

### 3.3. Outputs
Fastani provides an output file which is converted to phyllip matrix using the pairwise_distance_to_distance_matrix.py script.

The phyllip matrix is further converted to newick format using ```bionj_tree.R``` script.

The final visualization can be obtained by feeding this newick format in R using ```ggtree```.


## 4. Antimicrobial Resistance and Virulence Profiles Using ABRicate
ABRicate is a screening tool that can find antimicrobial resistance (AMR) or virulence genes. Given a DNA sequence, it uses a homology-based approach in finding these genes. It utilizes the Comprehensive Antibiotic Resistance Database and Virulence Factor Database for the reference sequences.

### 4.1. Installation
```conda install -c bioconda abricate```

### 4.2. Updating Database
Before analysis, the databases for the corresponding AMR and virulence profiles need to be downloaded. Use the --db option to download the available databases (i.e., card and vfdb).

* ```--db```: choose available database
* ```--force```: force download the latest version

```abricate-get_db --db <database> --force```

### 4.3. Usage

ABRicate takes in any contig file that is in FASTA format. It can take multiple sample files at once as well. Choose the appropriate database for the analysis of either AMR or virulence genes.

* ```--db```: choose available database

```abricate --db <database> /path/to/*.fasta > results.tab```

### 4.4. Outputs

ABRicate will output a ```results.tab``` file that contains information on the genes present, its coverage, and phenotype (i.e., antibiotic resistance or virulence). For a simple matrix to see which genes are present and absent, you can combine your results with the following command:

* ```--summary```: summary matrix of absent/present genes

```abricate --summary results.tab > summary.tab```

## 5. visualization of the phylogenetic trees
Com_gen_R.R script is used to visualize the NJ trees from the newick output files and integrating the metadeta into it.
### 5.1. Usage
```Rscript Com_gen_R.R metadata.txt kSNPtree CHEWtree STRINGtree ANItree ```


## References

 - [stringMLST](https://github.com/jordanlab/stringMLST/blob/master/README.md)
 - [chewBBACA](https://github.com/B-UMMI/chewBBACA)
 - [GrapeTree](https://github.com/achtman-lab/GrapeTree)
 - [plottree](https://pypi.org/project/plottree/)
 - [kSNP](https://sourceforge.net/projects/ksnp/files/)
 - [FastANI](https://github.com/ParBLiSS/FastANI)
 - [FastANI_heatmap](https://github.com/spencer411/FastANI_heatmap)
 - [ABRicate](https://github.com/tseemann/abricate)

