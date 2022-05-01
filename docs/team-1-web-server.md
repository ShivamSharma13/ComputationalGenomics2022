# Team1-WebServer

# Location: /home/groupa/webserver/Team1-WebServer/

####  MEMBERS:

Zack Mudge, Varsha Bhat, Shreyash Gupta, Palak Aggarwal, Mannan Bhola, Erin Connolly, Amartya Mandal

### This is a webserver named SALADS(SALmonella entericA preDiction and Subtyping) for analyzing *Salmonella enterica* isolates 

### The pipeline integrated in the backend is shown in this diagram:
<br></br>
<img src="imgs/Pipeline.png" width="1200">
<br></br>


####  TOOLS USED:

##### Genome Assembly
[SPAdes](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3342519/)
[QUAST](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3624806/)
[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

##### Gene Prediction
[Prodigal](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-11-119/)
[Glimmer3](http://ccb.jhu.edu/papers/glimmer3.pdf/)
[GeneMark-S2](https://pubmed.ncbi.nlm.nih.gov/29773659/)
[Infernal](https://academic.oup.com/bioinformatics/article/25/10/1335/270663/)
[bedtools](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2832824/)

#####  Functional Annotation
[eggNOG](https://doi.org/10.1093/molbev/msab293/)
[SignalP](https://www.nature.com/articles/s41587-021-01156-3/)
[TMHMM](https://services.healthtech.dtu.dk/service.php?TMHMM-2.0/)
[VFDB](https://doi.org/10.1093/nar/gkab1107/)
[Piler-CR](https://doi.org/10.1186/1471-2105-8-18/)

##### Comparative Genomics
[FastANI](https://www.nature.com/articles/s41467-018-07641-9/)

### The website's homepage is shown below:
<br></br>
<img src="imgs/screenshot.PNG" width="800">
<br></br>



#### The user has to provide paired-end FASTQ read files and their email ID (as they will receive a link to access their results) as input.
This will run the complete pipeline including Genome Assembly (GA), Gene Prediction (GP), Functional Annotation (FA), and Comparative Genomics (CG). The shell script activates each conda environment and runs the required part of the code (GA, GP, FA, CG) for every  job in the data_queue. The results will be stored in the data_results folder under the same job_id


### Outputs:

#### Genome Assembly
It provides the assembled contigs using SPAdes in the genome_assembly.zip

#### Gene Prediction
It provides the predicted genes taken in consensus using Prodigal, Glimmer3 and GeneMark-S2 in the gene_prediction.zip

#### Functional Annotation
It provides the functionally annotated genes using eggNOG, SignalP, TMHMM, VFDB and Piler-CR in the functional_annotation.zip

#### Comparative Genomics
It provides the results of the comparision of the user's isolates using fastANI with the 50 isolates in the comparative_genomics.zip

### Technical Stack
The technical stack implemented comprises:
* Django: Python web framework
* Gunicorn: Python web server gateway interface (WSGI) HTTP server
* NGINX: Reverse proxy
* Postgres: Database
