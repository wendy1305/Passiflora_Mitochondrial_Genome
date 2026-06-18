# 🧬 Pre-processing: Read Alignment and Filtering

This guide outlines the steps to filter Nanopore reads by aligning them to reference organelle genomes. The goal is to select mitochondrial reads while removing chloroplast contamination.

---

## 📍 Step 1: Read Alignment to Reference Mitochondrial Genome

Align your long reads to the reference mitogenome (*Passiflora organensis*) and extract only the reads that successfully mapped.

### 1. Run Minimap2 alignment
```bash
minimap2 -ax map-ont ref.fasta reads_ont.fastq | samtools view -Sb > alignment.bam
```

### 2. Extract and convert mapped reads using Samtools
```bash
# Keep only mapped reads (-F 4)
samtools view -b -F 4 alignment.bam > map_reference.bam 

# Sort the aligned reads
samtools sort map_reference.bam -o map_reference.sorted.bam

# Convert sorted BAM back to FASTQ format
samtools fastq map_reference.sorted.bam > mapped.fastq
```

---

## 📍 Step 2: Chloroplast Filtering (Remove Contamination)

Align the previously filtered reads to a reference chloroplast genome to identify and remove chloroplast sequences.

### 1. Run Minimap2 alignment
```bash
minimap2 -ax map-ont cpDNA.fasta mapped.fastq | samtools view -Sb > alignment.bam
```

### 2. Extract unmapped reads using Samtools
```bash
# Keep only unmapped reads (-f 4) to remove chloroplast contamination
samtools view -b -f 4 alignment.bam > nomap_cpDNA.bam 

# Sort the unmapped reads
samtools sort nomap_cpDNA.bam -o nomap_cpDNA.sorted.bam

# Convert sorted BAM to the final clean FASTQ file
samtools fastq nomap_cpDNA.sorted.bam > nomapped.fastq
```
