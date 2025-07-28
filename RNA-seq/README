# RNA-seq Analysis Workflow — Hertz et al.  
**Last updated: 20 March 2025**

This repository contains files and instructions to reproduce the RNA-seq read analysis pipeline used in Hertz et al. This includes read mapping with STAR, variant-specific read classification, and transcription termination assessment.

---

## 📁 File Descriptions

| File | Description |
|------|-------------|
| `Variant_Genome.fasta` | Full-length RNA transcript sequences from the oligo pool (used as a reference genome). |
| `LMH_1_GitHub.out.sam` | First 10,000 lines of STAR output for sample `LMH_1`. |
| `Read_Classification_SUBMIT.ipynb` | Jupyter notebook that parses `.out.sam` files, counts read lengths for each variant, and evaluates transcription termination. |

---

## 🧬 Building the STAR Genome Index  
**Date: 10 May 2025**

Follow the steps below to prepare the reference genome for STAR read mapping.

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
