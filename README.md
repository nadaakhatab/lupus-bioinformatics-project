# FASTQ Dataset Preparation for Lupus Autoimmune Analysis

## Project Overview

This project prepares RNA sequencing datasets for bioinformatics analysis of **Systemic Lupus Erythematosus (SLE)**, an autoimmune disease.
The dataset contains sequencing reads from healthy individuals and lupus patients. The data will be used for downstream bioinformatics analysis such as read statistics, visualization, and disease-related comparisons.

The sequencing data was obtained from the public repository **NCBI Sequence Read Archive**.

---

## Project Structure

```
bioinformatics_project/
│
├── data/
│   ├── healthy/
│   │   ├── SRR5413733_1.fastq
│   │   ├── SRR5413733_2.fastq
│   │   └── ...
│   │
│   └── disease/
│       ├── SRR5413805_1.fastq
│       ├── SRR5413805_2.fastq
│       └── ...
│
├── README.md
├── requirements.txt
└── .gitignore
```

### Folder Description

| File / Folder    | Description                                         |
| ---------------- | --------------------------------------------------- |
| data/            | Contains downloaded FASTQ sequencing datasets       |
| healthy/         | Samples from healthy individuals                    |
| disease/         | Samples from lupus patients                         |
| README.md        | Project documentation                               |
| requirements.txt | Python dependencies                                 |
| .gitignore       | Prevents unnecessary files from uploading to GitHub |


---

## Project Purpose

The goal of this project is to:

* Download and organize FASTQ sequencing datasets.
* Prepare a structured bioinformatics project environment.
* Separate samples into **Healthy vs Disease classes**.
* Enable downstream bioinformatics analysis and visualization.

This dataset can be used for RNA-seq analysis, read statistics, and potential disease classification tasks.

---

## Virtual Environment Setup

Create a Python virtual environment:

```
python -m venv bioinfo_env
```

Activate the environment:

Windows:

```
bioinfo_env\Scripts\activate
```

Install required libraries:

```
pip install -r requirements.txt
```

---

## Required Libraries

| Library    | Purpose                             |
| ---------- | ----------------------------------- |
| Biopython  | FASTQ parsing and sequence analysis |
| pandas     | Data handling                       |
| matplotlib | Data visualization                  |
| seaborn    | Statistical visualization           |
| scikit-bio | Bioinformatics analysis tools       |

---

## Important Notes

* The `.venv` or `bioinfo_env` folder is excluded using `.gitignore`.
* FASTQ files are not included in the repository due to their large size.
* Dataset accession IDs are provided so users can download the data directly from SRA.

---


## Dataset Source

Database: **NCBI Sequence Read Archive**

Project Accession: **PRJNA379992**

### Healthy Samples

* SRR5413733
* SRR5413734
* SRR5413735
* SRR5413736
* SRR5413737

### Disease (Lupus) Samples

* SRR5413805
* SRR5413806
* SRR5413807
* SRR5413808
* SRR5413809

/!\ Due to the large size of sequencing datasets, FASTQ files are **not uploaded to the GitHub repository**. They should be downloaded directly from the SRA database.




###Links

Project ID:
PRJNA379992
https://www.ncbi.nlm.nih.gov/bioproject/PRJNA379992/


Samples:

SRR5413733 - Healthy
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413733  

SRR5413734 - Healthy 
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413734

SRR5413735 - Healthy  
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413735

SRR5413736 - Healthy  
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413736

SRR5413737 - Healthy  
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413737

SRR5413805 - Disease 
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413805
https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578


SRR5413806 - Disease   
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413806
https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578

SRR5413807 - Disease   
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413807
https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578

SRR5413808 - Disease   
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413808
https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578

SRR5413809 - Disease 
https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413809
https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578

Source Link:
https://www.ncbi.nlm.nih.gov/sra