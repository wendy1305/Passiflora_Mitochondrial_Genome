# 🧬 Comparative Genomics Analysis

This guide describes the comparative genomics pipeline used to analyze and align mitochondrial genomes. The analysis compares our reference genome with three closely related study species using **MAUVE**.

---

## 📋 Dataset Summary

* **Reference Genome:** *Passiflora organensis*
* **Study Species:**
  * *Passiflora edulis*
  * *Passiflora alata*
  * *Passiflora haematostigma*

---

## 📍 Step 1: Prepare Input Files

Before running the alignment, ensure you have the genome files in **FASTA** or **GenBank (.gbk)** format. GenBank files are highly recommended as they allow mAUVE to display gene annotations visually.

```text
directory/
├── P_organensis_ref.gbk
├── P_edulis.gbk
├── P_alata.gbk
└── P_haematostigma.gbk
```

---

## 📍 Step 2: Genome Alignment with MAUVE

We use the progressiveMauve algorithm to identify large-scale evolutionary changes, such as inversions, translocations, and Locally Collinear Blocks (LCBs).

### Command Line Execution
If you are running the command-line version of MAUVE, use the following syntax to run the alignment:

```bash
progressiveMauve \
  --output=passiflora_comparison.mauve \
  --output-guide-tree=passiflora_tree.dnd \
  P_organensis_ref.gbk \
  P_edulis.gbk \
  P_alata.gbk \
  P_haematostigma.gbk
```

### Graphical User Interface (GUI) Execution
If you prefer using the MAUVE desktop application:
1. Open **MAUVE** on your computer.
2. Click on **File** > **Align with progressiveMauve...**
3. Click **Add Sequence** to load all four genome files.
4. Set *Passiflora organensis* as the top sequence (Reference).
5. Choose an output file directory and click **Align**.

---

## 📍 Step 3: Analysis of Results

Once the alignment is finished, analyze the interactive viewer to explore:

* **Locally Collinear Blocks (LCBs):** Regions of conserved sequence identity shared between the genomes.
* **Genome Rearrangements:** Look for blocks that are inverted (drawn upside down below the centerline) or shifted in position compared to *P. organensis*.
* **Sequence Insertions/Deletions:** Gaps within blocks that show unique regions present in one species but missing in others.
