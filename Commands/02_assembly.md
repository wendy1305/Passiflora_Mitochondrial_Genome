
# 🧬 Mitogenome Assembly and Polishing Workflow

This guide details the pipeline for assembling mitochondrial genomes using unmapped long reads and polishing the assembly with short reads.

---

## 📍 Step 1: Mitogenome Assembly
Use the unmapped data generated in `01_read_alignment.md` to assemble the genome. You can choose either **Canu** or **Flye**.

### Option A: Canu
```bash
canu -p assembly_canu -d canu_output genomeSize=1m maxThreads=16 -nanopore nomapped.fastq
```

### Option B: Flye
```bash
flye --nano-hq nomapped.fastq --genome-size 1m --min-overlap 1000 --threads 16 -o assembly_flye
```

---

## 📍 Step 2: First Round of Polishing (Pilon)
Improve your assembly quality by mapping short reads and running **Pilon**.

### 1. Index the assembly
```bash
bwa index assembly.fasta
```

### 2. Map the short reads
```bash
bwa mem -t 16 assembly.fasta R1.fastq R2.fastq > align.sam
```

### 3. Convert and sort with Samtools
```bash
samtools view -bS align.sam > align.bam
samtools sort align.bam -o align.sorted.bam
samtools index align.sorted.bam
```

### 4. Run Pilon
```bash
java -Xmx32G -jar pilon.jar --genome assembly.fasta --frags align.sorted.bam --output assembly_pilon --threads 16
```

---

## 📍 Step 3:  Second Round of Polishing
Repeat the polishing process using the output from the first round (`assembly_pilon.fasta`) to achieve higher accuracy.

### 1. Index the new assembly
```bash
bwa index assembly_pilon.fasta
```

### 2. Map the short reads to the new assembly
```bash
bwa mem -t 16 assembly_pilon.fasta R1.fastq R2.fastq > align_round2.sam
```

### 3. Convert and sort with Samtools
```bash
samtools view -bS align_round2.sam > align_round2.bam
samtools sort align_round2.bam -o align_round2.sorted.bam
samtools index align_round2.sorted.bam
```

### 4. Run Pilon again
```bash
java -Xmx32G -jar pilon.jar --genome assembly_pilon.fasta --frags align_round2.sorted.bam --output assembly_pilon_v2 --threads 16
```


