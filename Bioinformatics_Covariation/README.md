# R-scape Covariation Analysis Pipeline — Fluoride Riboswitch  
**Last updated: 21 August 2024**

This repository outlines the workflow used to create and analyze covariation models of the fluoride riboswitch, including FASTA preparation, alignment generation, and R-scape (v2.0.0.j) analysis.

---

## 📁 Input Files and Jupyter Notebooks

| File | Description |
|------|-------------|
| `20230524_RF01734.fa` | FASTA file downloaded from Rfam (May 24, 2023); example input for sequence prep. |
| `Prepping_sequences.ipynb` | Takes `20230524_RF01734.fa` and outputs an Excel file of oligos to order for synthesis. |
| `NCBI_CDS.ipynb` | Retrieves coding sequence (CDS) annotations from NCBI based on the oligos from `Prepping_sequences.ipynb`. |
| `CDS_ARNold.ipynb` | Sends sequences to the ARNold webserver to predict intrinsic termination and exports results. |
| `Shelley_Alignment.ipynb` | Prepares a full riboswitch alignment from CDS+ARNold data for use in R-scape. |
| `R-scape analysis 00README` | (This file) Documentation of the next stage: covariation analysis using [R-scape](http://eddylab.org/software/rscape/R-scape_userguide.pdf). |

---

## 📂 Key Files for R-scape Analysis

| File | Description |
|------|-------------|
| `ARNold_Fluoride_terminators.fasta` | Sequences predicted to terminate via intrinsic termination by ARNold. |
| `RF01734.sto` | Stockholm format alignment for the fluoride aptamer, downloaded from Rfam. |
| `Predicted_RF01734_profile.sto` | Alignment of predicted terminator-containing sequences to the fluoride aptamer. |
| `Predicted_Whole_Fluoride_Alignment.sto` | Stockholm format output of the full riboswitch alignment used in R-scape. |
| `Predicted_Whole_Fluoride_Alignment_1.cacofold_mod.sto` | First R-scape-processed alignment with gaps removed. |
| `Predicted_Whole_Fluoride_Alignment_1.cacofold_mod_apo.sto` | Structure annotated for the apo (ligand-free/terminator) state — matches Figure 1C. |
| `Predicted_Whole_Fluoride_Alignment_1.cacofold_mod_holo.sto` | Structure annotated for the holo (ligand-bound/aptamer) state — matches Figure 1B. |
| `Measured_Terminators.fasta` | FASTA file of experimentally validated fluoride riboswitches from NGS data. |

---

## 🧪 Terminal Commands for R-scape Analysis  
**Run date: 20 August 2024**

### 🔹 Align aptamers predicted to terminate
```bash
nhmmer -A Predicted_RF01734_profile.sto RF01734.sto ARNold_Fluoride_terminators.fasta
```
### Align predicted terminators
```python
Shelley_Alignment.ipynb
```
### 🔹 Covariation analysis of predicted alignment
```bash
R-scape --cacofold Predicted_Whole_Fluoride_Alignment.sto
```
### 🔹 R-scape analysis with apo and holo structures
```bash
R-scape -s --cacofold Predicted_Whole_Fluoride_Alignment_1.cacofold_mod_apo.sto
R-scape -s --cacofold Predicted_Whole_Fluoride_Alignment_1.cacofold_mod_holo.sto
```
