# Listeria Outbreak webserver
To create one stop site for Listeria with a user friendly interface, wherein the user can upload the fasta files and then perform genome assembly, prediction, functional annotation and comparative genomics. The tools for which have been chosen after careful consideration. Visualization is also offered in form of phylogenetic tree and US map with isolate demarcation (metadata has to be filled).

## URL to the Webserver
### http://groupc.bioappgenome22.biosci.gatech.edu/

## Technologies
### Front-end
1. **React** : 
   A JavaScript front-end library for building UI, Allows usto mix HTML with JS – JSX.

2. CSS - **Material UI** : 
   Simple, customizable, and accessible CSS library of React components.

3. **Node.js** : 
   Back-end JavaScript runtime environment that runs on the V8 engine.

4. **D3.js** : 
   JavaScript library for producing dynamic, interactive data visualizations.

### Back-end
1. **Flask** : 
   Micro web framework, lightweight good for routing, request handling and sessions with
   minimilastic features and high degree of customization.
  



## Architecture, Pipeline, Job Req. & Outputs
|||
|--|--|
|![alt text](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/blob/main/Web%20Server_%20Result%20Presentation/Slide2.PNG)|![alt text](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/blob/main/Web%20Server_%20Result%20Presentation/Slide3.PNG)|
|![alt text](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/blob/main/Web%20Server_%20Result%20Presentation/Slide4.PNG)|![alt text](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/blob/main/Web%20Server_%20Result%20Presentation/Slide5.PNG)|  


## Procedure  

1. Upload the fasta file and metadata file follow the direction show on the web
<img src="https://github.gatech.edu/storage/user/52307/files/17a2c99a-dc10-4a4a-a6d3-c58f804359b9" width="400">
2. Enter your email and we will email your result as soon as the pipeline has finished
<img src="https://github.gatech.edu/storage/user/52307/files/876a2ea9-850c-4b22-9e20-23456030e3db" width="400">

## Result

1. The result will be sent through a link to your Email  

2. The result will consist of 3 main components  
A Download button will provide all the results from our pipeline in .zip  

<img src="https://github.gatech.edu/storage/user/52307/files/0b903f4f-eb1f-4197-ad94-512f467b8a0c" width="400" >  

3. A US bubble interactive map that will show the outbreak based on the metadata will be shown on web  
The bubble of the outbrak indicate the number of isolates where larger bubble means more numbers of isolates  
Hower over the bubble will show a detailed isolates list with top outbreak food sources 

<img src="https://github.gatech.edu/storage/user/52307/files/042eac08-d845-4d7a-a0eb-e7ee04ef5ce7" width="400" >

4. A phylogenetic tree of the isolates will be displayed on webpage!  

<img src="https://github.gatech.edu/storage/user/52307/files/5328b726-adf0-45b3-b6d0-fcc483a693e0" width="400">

## For Local Setup

1. Clone all the repos required to run Team3-Webserver using:

   ```
   git clone https://github.gatech.edu/computationalgenomics2022/Team3-WebServer
   git clone https://github.gatech.edu/computationalgenomics2022/Team3-ComparativeGenomics
   git clone https://github.gatech.edu/computationalgenomics2022/Team3-GenePrediction
   git clone https://github.gatech.edu/computationalgenomics2022/Team3-GenomeAssembly
   ```
2. Then navigate to [backend](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/tree/main/backend) using:
   ```
   cd Team3-WebServer/backend
   ```

3. Create conda env and install these (from the backend folder):

   ```
   conda config --add channels bioconda
   conda config --add channels conda-forge
   conda create -n webserver-env2
   conda install -n webserver-env2 --file webserver-env2.txt
   conda create -n webserver-env2.2
   conda install -n webserver-env2.2 --file webserver-env2.2.txt
   conda activate webserver-env2
   ``` 
   This will install [Node.js](https://nodejs.org/en/) and other required packages
4. To run the backend:
   ```
    python app.py
   ```

5. In a different terminal navigate to [frontend](https://github.gatech.edu/computationalgenomics2022/Team3-WebServer/tree/main/frontend) and activate the conda env:
    ```
    cd ..
    cd frontend
    conda activate webserver-env2
    ```
6. Install node_modules  
    ```
    npm install 
    ```
7. Begin the frontend web application by typing  
    ```
    npm start 
    ```
8. To the job processing, open yet another terminal and execute following:  
    ```
     cd Team3-WebServer/backend
     conda activate webserver-env2
     python process_jobs.py
    ```

8. All set, Our frontend and backend use port 3000 and 5000 respectively.

9. Go to http://localhost:3000/, and explore!

## Members  
|||||||||
|--|--|--|--|--|--|--|--|
|Ryan Abramowitz|Arvind Bangaru|Dhruv Karve|Colin Naughton|Haojun Song|Nidhi Koundinya|Haoting Chen|Piyus Mohanty|  
