# Step 1: Read alignment to reference mitochondrial genome  ( *Passiflora organensis*)

### Minimap2 alignment
```bash
minimap2 -ax  map-ont ref.fasta reads_ont.fastq | samtools view -Sb > alignment.bam
```
### Samtools 

```bash
samtools view -b -F 4 alignment.bam > map_reference.bam 
samtools sort map_reference.bam -o map_reference.sorted.bam
samtools fastq map_reference.sorted.bam  > mapped.fastq
```

# Step 2: Read alignment mapped to reference chloroplast genome  ( Clean chloroplast seqeuences)

### Minimap2 alignment
```bash
minimap2 -ax  map-ont cpDNA.fasta mapped.fastq | samtools view -Sb > alignment.bam
```
### Samtools 

```bash
samtools view -b -f 4 alignment.bam > nomap_cpDNA.bam 
samtools sort nomap_cpDNA.bam  -o nomap_cpDNA.sorted.bam
samtools fastq map_reference.sorted.bam  > mapped.fastq
