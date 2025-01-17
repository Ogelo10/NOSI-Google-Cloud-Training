# Consensus Pathway Analysis using Google Cloud Infrastructure

This Cloud-based learning module teaches Pathway Analysis, a term that describes the set of tools and techniques used in
life sciences research to discover the biological mechanism behind a condition from high throughput biological data. 
Pathway Analysis tools are primarily used to analyze these omics datasets to detect relevant groups of genes that are 
altered in case samples when compared to a control group. Pathway Analysis approaches make use of already existing pathway 
databases and given gene expression data to identify the pathways which are significantly impacted in a given condition.

The course is structured such that the content will be arranged in five Sub-Modules which allows us to:
1. Download and process data from public repositories,
2. Perform differential analysis,
3. Perform pathway analysis using different methods that seek to answer different research hypotheses,
4. Perform meta-analysis and combine methods and datasets to find consensus results, and
5. Interactively explore significantly impacted pathways across multiple analyses, and browsing relationships between 
pathways and genes.


## Overview of Page Contents


+ [Getting Started](#GS)
+ [Overview](#OV)
+ [Google Cloud Buckets](#BUCKET)
+ [Workflow Diagrams](#WORK)
+ [Google Cloud Architecture](#ARCH)
+ [Software Requirements](#SOF)
+ [Data](#DATA)
+ [TroubleShooting](#TR)
+ [Funding](#FUND)
+ [License](#LIC)


## <a name="GS">Getting Started</a>
Each learning submodule will be organized in a R Jupyter notebook to help the participants familiarize themselves 
with the cloud computing in the specific context of running bioinformatics workflows. Each notebook will include 
step-by-step hands-on practice with R command line to install necessary tools, obtain data, perform analyses, visualize 
and interpret the results.


## <a name='OV'>Overview</a>

The content of the course is organized in R Jupyter Notebooks. Then we use Jupyter Book which is a package to combine 
individuals Jupyter Notebooks into a web-interface for a better navigation. Details of installing the tools and formatting
the content can be found at: https://jupyterbook.org/en/stable/intro.html. The content of the course is reposed in the 
Github repository of Dr. Tin Nguyen's lab, and can be found at https://github.com/tinnlab/NOSI-Google-Cloud-Training.
The overall idea of the modules are explained below:

+ [**Sub-Module 1**](./Module01-GEO_Data_Processing.ipynb) describes how to obtain data from public repository, process and save the expression matrix and shows how to map probe IDs into gene symbols.
+ [**Sub-Module 2**](./Module02-DE_Analysis.ipynb) focuses on Differential Expression Analysis using `limma`, `t-test`, 
`edgeR`, and `DESeq2`.
+ [**Sub-Module 3**](./Module03-Gene_Set_and_Pathway.ipynb) introduces common curated biological databases such as Gene Ontology (GO), Kyoto Encyclopedia of Genes and 
Genomes (KEGG)
+ [**Sub-Module 4**](./Module04-Pathway_Analysis.ipynb) aims at performing Enrichment Analysis methods using popular methods such as `ORA`, `FGSEA`, and `GSA`.

## <a name="BUCKET">Creating Google Cloud Storage Buckets</a>
In this section,we will describe the steps to take to create Google Cloud Storage Buckets to store our data.
This can be achieved using GUI or using the command line.
To use the GUI, the user has to first visit the url https://console.cloud.google.com/storage/,  sign in, click on buckets on the left menu bar.
# <img src="./images/Bucket/Step0.png">

Next, the user has to click on the "CREATE" button below the search bar.
# <img src="./images/Bucket/Step1.png">

This will then open the page where the user will supply the name of the bucket, the
location, access control and other information about the bucket. After this the user will 
click on the "CREATE" button to complete the process.
# <img src="./images/Bucket/Step2.png">

However, to create a Bucket using the command line, the user can use the gcloud storage buckets `create` command
`gcloud storage buckets create gs://BUCKET_NAME` where `BUCKET_NAME` is the user-defined name. 
If the request succeeds, the user gets a success message. The user can also add optional flags
while running the `create command ` to have greater control over the creation of the bucket.
Such flags include `--project: PROJECT_NAME`, `--default-storage-class: STORAGE_CLASS`, `--location: LOCATION`
and `--uniform-bucket-level-access` with `PROJECT_NAME` and `STORAGE_CLASS` supplied by the user.

Storage Buckets can also be created on the command line using the `gsutils mb` command.
The command to do so is `gsutil mb gs://BUCKET_NAME`, with `BUCKET_NAME` the desired bucket name.
This command also returns a success message upon completion and can also take optional flags 
`-p`, `-c`, `-l`, `-b` and their user-supplied values, corresponding to project ID or number, default storage class, location of the bucket and 
uniform bucket-level access respectively, just like the `create` command.

## <a name="WORK">Workflow Diagrams</a>

# <img src="./images/Intro/Main-img.png" width="900" height="700">

As seen in the image above, we will show how to download sequence data from sources such as the GEO repository using the
GEO website as well as the GEOquery package from the R programming language terminal.
We will use the getGEO function of the package to download GEO datasets, as identified by the accession ID. 
Exploration and preprocessing of the data follows, after which we export it to a desired format.
Next, we map the probe set IDs of the datasets to Entrez gene ID to achieve uniformity, before carrying out Differential
Gene Expression Analysis on the data, which is the focus of Module 2.
It involves assigning samples into groups and setting up design matrix, and then performing DE analysis using limma, t-test, 
edgeR and DESeq packages to produce results which are filtered and exported before being further visualized.

Module 3 introduces Ontology, Gene Ontology and the KEGG Pathway Database. Also, terms and pathway gene sets are downloaded 
from GO and KEGG and then the results are saved to the GMT file format. In Module 4, some Pathway Analysis methods are introduced,
before Meta analysis methods are discussed in Module 5. 

## <a name="ARCH">Google Cloud Architecture</a>

# <img src="./images/Intro/architecture.png" width="900" height="500">
The figure above shows the architecture of the learning module with Google Cloud infrastructure. First, we will create
an VertexAI workbench with R kernel. The code and instruction for each Sub-Module is presented in a separate Jupyter Notebook.
User can either upload the Notebooks to the VertexAI workbench or clone from the project repository. Then, users can execute 
the code directly in the Notebook. In our learning course, the Sub-Module 1 will download data from the public repository (e.g. GEO database)
for preprocessing and save the processed data to a local file in VertexAI workbench and to the user's Google Cloud Storage Bucket. The output
of the Sub-Module 1 will be used as inputs for all other Sub-Modules. The outputs of the Sub-Modules 2, 3, and 4 will be saved to 
local repository in VertexAI workbench and the code to copy them to the user's Cloud Bucket is also included.

# <a name="SOF">Software Requirements</a>
This learning Module does not require any computational hardware and local environment setting from users as the 
programs and scripts can be run in the browser-based development environment provided by Google. However, 
users need to have Google email account, sufficient internet access, and a standard web-browser (e.g. Chrome, Edge, 
Firefox etc., Chrome browser is recommended) to create a Cloud Virtual Machine for analysis.

The Jupyter Book will run and test all codes in our notebooks and then build them into a website format. 
The website output is the `_build/html` folder in the repository. After the `_build/html` is completely built, the web 
version of the book will be uploaded from the `_build/html` folder to a Google bucket. 
The address of the cloud bucket can be found at: https://storage.googleapis.com/nosi-gcloud-course/html/intro.html. 
The link http://tinnguyen-lab.com/course/cpa-gcloud/intro.html is just a proxy to that link using Dr. Tin Nguyen's Lab
domain name. It is recommended to execute the Jupyter Note Book using R kernel version > 4.1 using a standard machine 
with minimum configuration of 4 vCPUs, 15 GB RAM, and 10GB of HDD.

A complete workflow from setting up the environment, building the book, to uploading the website to 
Google cloud bucket can be found here: https://github.com/tinnlab/NOSI-Google-Cloud-Training/blob/main/.github/workflows/main.yml. 
The logs for each time it builds the book are here: https://github.com/tinnlab/NOSI-Google-Cloud-Training/actions.

## <a name="DATA">Data</a>
All data from the modules were originally downloaded from the Gene Expression Omnibus (GEO) repository using the accession
number GSE48350 file. The data was originally generated by **Berchtold and Cotman, 2013**. We preprocessed this data and
normalized it, after which we used it in the subsequent analyses.

## <a name="TR">Troubleshooting</a>

Prior to publishing, some common errors frequently encountered by the early users in the workflow include syntax errors
such as calling a library that has not been initialized.
Also, some users who were still beginners to Bioinformatics made basic errors such as confusing the meanings of terms such as 
Pathways and Gene Sets, and misunderstanding what these stand for. However, this is an error that is attributed to limited 
knowledge and is cleared by exposure to the first few sections of Sub-Module 1.

## <a name="FUND">Funding</a>

This work was fully supported by NIH NIGMS under grant number GM103440. Any opinions, findings, and conclusions, 
or recommendations expressed in this material are those of the authors and do not necessarily reflect the views 
of any of the funding agencies.

## <a name="LIC">License for Data</a>

Text and materials are licensed under a Creative Commons CC-BY-NC-SA license. The license allows you to copy, remix 
and redistribute any of our publicly available materials, under the condition that you attribute the work (details in the license)
and do not make profits from it. More information is available [here](https://tilburgsciencehub.com/about/#license).

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" 
style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />

This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative 
Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.






