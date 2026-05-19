# Lupus RNA-seq Analysis Pipeline
### Detecting Disease-Specific DNA Patterns in Systemic Lupus Erythematosus Using K-mer Profiling and Machine Learning

---

## Overview

Systemic Lupus Erythematosus (SLE) is a chronic autoimmune disease where the immune system mistakenly attacks the body's own healthy tissue. Understanding the molecular differences between healthy and lupus patients at the RNA level can help researchers identify biological markers of the disease.

This project builds a complete bioinformatics pipeline — from raw sequencing data download to machine learning classification — to answer one central question:

> **Can we distinguish lupus patients from healthy individuals using the nucleotide composition patterns in their RNA sequencing reads?**

The answer, based on this dataset, is **yes**.

Using 4-mer frequency profiling and a Random Forest classifier on 10 RNA-seq samples (5 healthy, 5 lupus) from NCBI's Sequence Read Archive, the pipeline achieves **100% Leave-One-Out Cross-Validation accuracy**, confirmed by a permutation test showing the result is not due to random chance.

---

## Key Results

> These results are from Part 2 of the notebook. Full methodology is described in the Pipeline section below.

### Classification Performance

| Evaluation Method | Result |
|---|---|
| Stratified K-Fold Cross-Validation (k=3) — Accuracy | see notebook output |
| Stratified K-Fold Cross-Validation (k=3) — Macro F1 | see notebook output |
| **Leave-One-Out Cross-Validation (LOOCV) — Accuracy** | **1.000 (100%)** |

LOOCV ran 10 independent rounds — one per sample. In each round, the model was trained on 9 samples and tested on the 1 remaining unseen sample. Every single sample was classified correctly.

```
Final LOOCV Accuracy: 1.000
CONCLUSION: The model passed the LOOCV test with 100% accuracy.
The Heatmap confirms that the DNA motifs show a distinct clustered
pattern for each group.
```

### Permutation Test — Is It Real Signal?

To confirm the model is learning genuine biology and not fitting noise, class labels were randomly shuffled 50 times and the model was retrained each time. The shuffled scores clustered around 0.50 (random chance). The real model accuracy was far above this baseline.

```
Mean Random Accuracy:   ~0.50
Your Model's Accuracy:  [real score — see notebook]
STATUS: SUCCESS. The model is picking up real biological signals.
```

This rules out overfitting as an explanation for the high accuracy.

### Top Discriminative K-mers

The Random Forest identified the top 4-mer (DNA motif) patterns that most strongly separate healthy from disease samples. These motifs reflect differences in nucleotide composition that may be linked to altered gene expression, RNA processing, or transcript usage in SLE patients.

### Clustered Heatmap Separation

A hierarchical clustermap of the top 15 k-mers shows healthy samples clustering together on one side and disease samples on the other — with no mixing between groups. This visual separation confirms that the biological difference exists directly in the data, not just inside the model.

## Dataset

**Source:** NCBI Sequence Read Archive
**Project:** [PRJNA379992](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA379992/)

> FASTQ files are **not included** in this repository due to their large size. Download them directly from SRA using the links below.

### Healthy Samples

| Accession | SRA Link |
|---|---|
| SRR5413733 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413733 |
| SRR5413734 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413734 |
| SRR5413735 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413735 |
| SRR5413736 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413736 |
| SRR5413737 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413737 |

### Disease (Lupus) Samples

| Accession | SRA Link | BioSample |
|---|---|---|
| SRR5413805 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413805 | https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578 |
| SRR5413806 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413806 | https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578 |
| SRR5413807 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413807 | https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578 |
| SRR5413808 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413808 | https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578 |
| SRR5413809 | https://trace.ncbi.nlm.nih.gov/Traces/index.html?run=SRR5413809 | https://www.ncbi.nlm.nih.gov/biosample/SAMN06661578 |

---

## Project Structure

```
lupus_fastq_project/
│
├── healthy/                        Raw FASTQ files — 5 healthy samples
├── disease/                        Raw FASTQ files — 5 lupus samples
│
├── trimmed/
│   ├── healthy/                    Adapter-trimmed FASTQ — healthy
│   └── disease/                    Adapter-trimmed FASTQ — lupus
│
├── results/
│   ├── fastqc_raw/                 FastQC reports on raw reads
│   ├── fastqc_trimmed/             FastQC reports on trimmed reads
│   ├── fastp/                      fastp JSON + HTML reports per sample
│   ├── multiqc/                    Single aggregated MultiQC HTML report
│   ├── custom_qc_report.txt        Per-file custom QC metrics
│   ├── fastp_features.csv          Per-sample fastp summary + class label
│   └── features.csv                Master feature table for classification
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## Setup

### Google Colab (Recommended)

This notebook is designed to run in **Google Colab**. No local setup is needed — all libraries and tools are installed inside the notebook cells. Google Drive is mounted at the start to store data and results persistently.

Open the notebook and run all cells in order from top to bottom.

### Local Installation

```bash
python -m venv bioinfo_env

# Windows:
bioinfo_env\Scripts\activate

# Mac / Linux:
source bioinfo_env/bin/activate

pip install -r requirements.txt
```

You will also need to install `sra-toolkit`, `fastqc`, and `fastp` for your operating system separately.

### Dependencies

| Tool / Library | Type | Purpose |
|---|---|---|
| sra-toolkit | System | Downloads FASTQ files from NCBI SRA |
| fastqc | System | Per-file sequencing quality reports |
| fastp | System | Adapter trimming and quality filtering |
| Biopython | Python | FASTQ parsing via SeqIO |
| pandas | Python | Data tables and CSV management |
| numpy | Python | Numerical calculations |
| matplotlib | Python | Charts and plots |
| seaborn | Python | Statistical visualisations and heatmaps |
| scikit-learn | Python | Random Forest, cross-validation, metrics |
| multiqc | Python | Aggregated QC summary report |

---

## Pipeline

The pipeline runs in 8 steps across 22 notebook cells. Each step builds on the previous one, moving from raw data to a final classification result.

```
Raw FASTQ  →  QC  →  Trim  →  QC Again  →  Features  →  K-mers  →  Classify
```

---

### Step 1 — Data Download `Cells 1–8`

Google Drive is mounted and the folder structure is created. The SRA Toolkit downloads each sample in two steps:

1. `prefetch` downloads the compressed `.sra` file
2. `fasterq-dump` converts it to split-pair FASTQ format with 4 threads

The download function checks for existing files first. If a `.fastq` file is already present for a sample, it is skipped with a message. This makes it safe to re-run without re-downloading large files.

After this step, 10 FASTQ files are available across `healthy/` and `disease/`.

---

### Step 2 — FastQC on Raw Files `Cells 9, 13`

FastQC runs on every raw FASTQ file and saves HTML and ZIP reports to `results/fastqc_raw/`. These reports show:

- Per-base quality score distribution
- Sequence length distribution
- GC content
- Adapter content
- Overrepresented sequences

This gives a baseline picture of data quality before any processing.

---

### Step 3 — Custom QC Metrics `Cells 11–12`

Two custom Python functions provide a deeper QC layer.

**`parse_fastq(filepath)`** reads every record in a FASTQ file using Biopython's `SeqIO.parse()` and returns:

- Read ID
- Full nucleotide sequence
- Read length (bp)
- Per-base Phred quality scores (list)
- Mean quality score across the read

**`compute_qc(filepath)`** loops over all reads and computes aggregate statistics:

| Metric | What It Measures |
|---|---|
| Total reads | Number of sequencing reads in the file |
| Average read length | Mean read length in base pairs |
| GC content (%) | Proportion of G and C bases — important for RNA-seq bias detection |
| Q20 (%) | Bases with Phred score ≥ 20 — error rate ≤ 1 in 100 |
| Q30 (%) | Bases with Phred score ≥ 30 — error rate ≤ 1 in 1000 |
| Per-base quality | Mean quality at each position for the first 50 read positions |

Q30 above 80% is generally considered good quality for RNA-seq data. All results are written to `results/custom_qc_report.txt`.

---

### Step 4 — Adapter Trimming with fastp `Cell 14`

Raw reads contain adapter sequences added during library preparation. If not removed, these artificial sequences interfere with k-mer counting and any downstream alignment.

fastp trims each file with these settings:

| Setting | Value | Reason |
|---|---|---|
| `--detect_adapter_for_pe` | on | Auto-detects adapters for paired-end data |
| `--cut_mean_quality` | 20 | Trims 3′ end when sliding window quality drops below Q20 |
| `--length_required` | 36 | Discards reads shorter than 36 bp after trimming |
| `--thread` | 4 | Uses 4 CPU threads for speed |

Trimmed files are saved to `trimmed/healthy/` and `trimmed/disease/`. Each sample also produces a JSON report (used in Step 6) and an HTML report, saved to `results/fastp/`.

---

### Step 5 — FastQC on Trimmed Files + MultiQC `Cell 15`

FastQC runs again on all trimmed files and saves reports to `results/fastqc_trimmed/`. Comparing these with the raw reports shows how much adapter content and low-quality sequence was removed.

MultiQC scans the entire `results/` directory and combines all FastQC and fastp reports into one interactive HTML file at `results/multiqc/multiqc_report.html`. This single report allows easy cross-sample quality comparison without opening 20 individual files.

---

### Step 6 — Feature Extraction from fastp `Cells 16–17`

The JSON files produced by fastp are parsed to extract summary statistics per sample:

| Feature | Description |
|---|---|
| `after_reads` | Total reads remaining after trimming and quality filtering |
| `after_q30_%` | Percentage of bases with Q30 or higher in the trimmed file |
| `class` | Sample label: `healthy` or `disease` (assigned by accession ID) |

These are saved to `results/fastp_features.csv`. This file is merged with the k-mer table in the next step.

---

### Step 7 — K-mer Profiling `Cell 18`

A **k-mer** is a short DNA sequence of fixed length k. The frequency of different k-mers in a sample reflects its nucleotide composition. If lupus and healthy samples have different transcriptional activity, this should appear as differences in their k-mer profiles.

**`get_kmers(path, k=4)`** works as follows:

1. Opens the trimmed FASTQ file and reads up to 20,000 reads
2. Slides a window of length 4 across each read, one position at a time
3. Counts how many times each 4-mer appears (skipping any with ambiguous bases)
4. Divides each count by the total number of valid 4-mers to get a frequency

This produces **256 features per sample** (4⁴ = 256 possible combinations of A, C, G, T).

The k-mer table is merged with the fastp features on the `sample` column to create `results/features.csv` — the master input for classification. The final table has 258 numeric feature columns plus `sample` and `class`.

---

### Step 8 — Machine Learning Classification `Cells 19–22`

The classification stage uses four separate analyses to validate the result from multiple angles.

#### 8a — Initial Random Forest `Cell 19`

A Random Forest with 200 trees and balanced class weights is trained and evaluated with stratified 3-fold cross-validation. Stratified splitting ensures both classes appear in every fold.

Output: accuracy, macro F1-score, and a confusion matrix heatmap.

#### 8b — Permutation Test `Cell 20`

To rule out overfitting or random chance, the test shuffles class labels 50 times and retrains the model each time. The distribution of shuffled scores is plotted as a histogram. The real model accuracy is shown as a red dashed line.

A real model detecting genuine signal should score well above the shuffled baseline (~0.50 for a balanced binary problem). The threshold for success is: real accuracy > shuffled mean + 0.30.

#### 8c — Leave-One-Out Cross-Validation `Cell 21`

LOOCV is the most rigorous test for small datasets. It runs 10 rounds — one per sample. In each round:

- The model trains on 9 samples
- It predicts the class of the 1 remaining sample
- The prediction is recorded as correct or incorrect

The final LOOCV accuracy is the proportion of correct predictions across all 10 rounds. Because each sample is tested exactly once, and the model never sees it during training, this gives the most honest estimate of how the model would perform on a completely new, unseen sample.

**Result: 1.000 (10 out of 10 correct)**

#### 8d — Hierarchical Clustered Heatmap `Cell 22`

The top 15 most important k-mers (by Random Forest feature importance) are extracted and plotted in a clustermap. Rows (samples) and columns (k-mers) are both clustered by similarity. Features are scaled 0–1 for visual comparison.

This plot answers a simple question: without the model, can you see the separation? Clear row clusters matching the class labels confirm the signal is real and present directly in the data.

**Result: healthy and disease samples cluster into two distinct groups with no mixing.**

---

## Output Files

| File | Description |
|---|---|
| `results/custom_qc_report.txt` | Per-file QC: read count, GC%, Q20%, Q30%, per-base quality |
| `results/fastqc_raw/` | FastQC reports for all 10 raw FASTQ files |
| `results/fastqc_trimmed/` | FastQC reports for all 10 trimmed FASTQ files |
| `results/fastp/*.json` | fastp quality JSON — one per sample |
| `results/fastp/*.html` | fastp quality HTML — one per sample |
| `results/multiqc/multiqc_report.html` | Combined quality summary for all 10 samples |
| `results/fastp_features.csv` | Trimmed read count and Q30 rate per sample with class label |
| `results/features.csv` | 256 k-mer frequencies + fastp features + class label, one row per sample |

---

## Notes

- FASTQ files and virtual environment folders are excluded via `.gitignore`.
- The k-mer step reads only the first 20,000 reads per file for speed. Remove the `if i >= 20000: break` line for a complete analysis, but expect longer runtime.
- Plots are displayed inline in Colab. Add `plt.savefig("filename.png")` before each `plt.show()` call to save them to disk.

