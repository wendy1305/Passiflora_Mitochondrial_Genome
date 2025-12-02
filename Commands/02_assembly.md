# Step 1: Assembly mitogenomes with nomapped data from 01_read_alignment.md

### Canu 
```bash
canu -raw -nanopore -d nomapped.fastq -p assembly_canu  genomeSize=1m  maxThreads=16 
```

### Flye

```bash
flye --nano-hq  nomapped.fastq  --genome-size 1m --min-overlap 1000 --threads 16  -o assembly_flye
```

# Step 2 : Polish with pilon 

### Index the assembly
```bash
bwa index assembly.fasta
```

### Map the reads
```bash
bwa mem -t 16 assembly.fasta R1.fastq R2.fastq > align.sam
```
### Samtools 
```bash
samtools view -bS align.sam > align.bam
samtools sort align.bam -o align.sorted.bam
samtools index align.sorted.bam
```

### Run Pilon 

```bash
java -Xmx32G -jar pilon.jar --genome assembly.fasta  --frags align.sorted.bam --output assembly_pilon  --threads 16
```

### Step 3 : Polish second round 

### Index the assembly
```bash
bwa index assembly_pilon.fasta
```

### Map the reads
```bash
bwa mem -t 16 assembly.fasta R1.fastq R2.fastq > align.sam
```
### Samtools 
```bash
samtools view -bS align.sam > align.bam
samtools sort align.bam -o align.sorted.bam
samtools index align.sorted.bam
```

### Run Pilon 

```bash
java -Xmx32G -jar pilon.jar --genome assembly.fasta  --frags align.sorted.bam --output assembly_pilon  --threads 16
```
