---
title: Slurm Script Template
layout: post
---

Slurm Script template using shifter. The example in the script converts BAM files to BigWig format. It sorts the BAM files in the folder "bam_files" to create an index file (.bai) for each BAM file using samtools and then used "bamCoverage" to convert the files to bigWig format.

```
#!/bin/bash

HOMESOURCE="source ~/.bashrc"   
SLURMPARTITION="blade,himem,hugemem" 
SHIFTER="shifter --image=hub.age.mpg.de/bioinformatics/software:v2.0.6 /bin/bash" 


top=$(readlink -f ../)/
tmp=$(readlink -f ../tmp)/
logs=$(readlink -f ../slurm_logs)/
bams=$(readlink -f ../bam_files)/
bw=$(readlink -f ../bigwig_output)/

cd ${bams} 

for file in $(ls *.bam ); do 
  rm -rf ${logs}${file%.bam}.bw.*.out
  echo ${file}
  
sbatch --partition $SLURMPARTITION << EOF
#!/bin/bash
#SBATCH --output ${logs}${file%.bam}.bw.%j.out
#SBATCH -c 4
#SBATCH --job-name='bam_to_bw'

module load shifter

${SHIFTER} << SHI
#!/bin/bash
${HOMESOURCE}

module unload python/2.7.15
module load python/3.6.5
module load samtools

cd ${bams}
echo ${bams}${file}

samtools sort ${file} -@ 18 > ${bams}${file%.bam}_sorted.bam
samtools index ${bams}${file%.bam}_sorted.bam > ${bams}${file}.bai
bamCoverage --binSize 5 --effectiveGenomeSize 2913022398 --normalizeUsing RPGC --bam ${file} \
            --outFileName ${bw}${file%.bam}.bigwig --outFileFormat bigwig
SHI
EOF
done

```




