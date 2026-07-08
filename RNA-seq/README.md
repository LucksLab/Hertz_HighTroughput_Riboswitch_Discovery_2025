# RNA-seq Analysis Workflow — Hertz et al.  
**Last updated: 20 March 2025**

This repository contains files and instructions to reproduce the RNA-seq read analysis pipeline used in Hertz et al. This includes read mapping with STAR, variant-specific read classification, and transcription termination assessment.

Raw sequencing files were uploaded to the NCBI BioProject with the ID: PRJNA1359411.

---

## 📁 File Descriptions

| File | Description |
|------|-------------|
| `Variant_Genome.fasta` | Full-length RNA transcript sequences from the oligo pool (used as a reference genome). |
| `LMH_1_GitHub.out.sam` | First 10,000 lines of STAR output for sample `LMH_1`. |
| `Read_Classification_RegExp.ipynb` | Jupyter notebook that parses `.out.sam` files, counts read lengths for each variant, and evaluates transcription termination based on a polyU Regular Expression (pattern = r'TTTTT\|T[AGC]TTTT\|TT[AGC]TTT\|TTT[AGC]TT'). |
| `Read_Classification_BacTermFinder.ipynb` | Jupyter notebook that parses `.out.sam` files, counts read lengths for each variant, and evaluates transcription termination based on output from BacTermFinder. |
| `BacTermFinder_Processing.ipynb` | Jupyter notebook that parses `out*.fasta_mean.csv` file from BacTermFinder. |

---

## 💻 Computational Environment

| Software | Version | 
|------|-------------|
| `Python` | 3.10.9 |
| `Jupyter` | 6.5.7 |
| `Biophython` |  1.78 |
| `Pandas` |  2.2.3 |
| `PEAR` | 0.9.6 |
| `STAR` | 2.7.9a |
| `MEGAHIT` | 1.0.6.1 |
| `BacTermFinder` | 1.0 |
| `Microscoft Excel` |  16.110.2 |
| `re` |  2.2.1 |

pear-env:
| Software | Version | 
|------|-------------|
| `_libgcc_mutex` | 0.1 |
| `_openmp_mutex` | 4.5 |
| `bzip2` |  1.0.8 |
| `libgcc-ng` |  13.2.0 |
| `libgomp` | 13.2.0 |
| `libzlib` | 1.2.13 |
| `pear` | 0.9.6 |
| `zlib` | 1.2.13 |

---

## 🧬 Building the STAR Genome Index  
**Date: 10 May 2025**

Follow the steps below to prepare the reference genome for STAR read mapping.

BacTermFinder: Taheri Ghahfarokhi, S. M. A., & Peña-Castillo, L. (2025). BacTermFinder: a comprehensive and general bacterial terminator finder using a CNN ensemble. NAR Genomics and Bioinformatics, 7(1), lqaf016.

### 1. Assemble Genome (Optional: if input FASTA is fragmented)
```bash
module load megahit/1.0.6.1
megahit -o megahit_Variant_genome_directory -r Variant_Genome.fasta
```
### 2. Generate STAR Genome Index
```bash
module load STAR/2.7.9a
STAR --runMode genomeGenerate \
     --genomeDir megahit_Variant_genome_directory \
     --genomeFastaFiles Variant_Genome.fasta \
     --genomeSAindexNbases 8
```
