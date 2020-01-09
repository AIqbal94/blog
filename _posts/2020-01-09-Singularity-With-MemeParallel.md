---
layout: post
title: Running MemeParallel with Singularity Template
---

A template for running MEME in parallel on Singularity. (Useful for running MEME on large datasets)

```
#!/bin/bash

top=$(readlink -f ../)/

fasta_up=$(readlink -f ../promoters.up.fa)
fasta_down=$(readlink -f ../promoters.down.fa)
background_up=$(readlink -f ../promoters.up.control.fa)
background_down=$(readlink -f ../promoters.down.control.fa)

A=$(readlink -f ../meme_output_clean_promoters)/
up_path=${A}up/
down_path=${A}down/
psp_path=$(readlink -f ../meme_output_clean_promoters/psp_gen_output/)/

mkdir -p ${A} ${up_path} ${down_path} ${psp_path}up ${psp_path}down

singularity exec /beegfs/common/singularity/bioinformatics_software.v3.0.0.sif /bin/bash << SIN
#!/bin/bash
source ~/.bashrc
module load meme
export PERL5LIB=/modules/software/meme/5.1.0/lib/perl5:/home/mpiage/.software_container/.perl/5.28.1/lib/perl5
export OMPI_MCA_btl_base_warn_component_unused=0

echo "1"
#meme -oc ${up_path} ${fasta_up} -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -revcomp

echo "2"
#meme -oc ${down_path} ${fasta_down} -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -revcomp

echo "3"
#/modules/software/meme/5.1.0/libexec/meme-5.1.0/psp-gen -pos ${fasta_up} -neg ${background_up} > ${psp_path}up/N2.vs.NFYB1.up.psp

echo "3b"
#/modules/software/meme/5.1.0/libexec/meme-5.1.0/psp-gen -pos ${fasta_down} -neg ${background_down} > ${psp_path}down/N2.vs.NFYB1.down.psp

echo "4"
#meme -oc ${psp_path}up/meme ${fasta_up} -psp ${psp_path}up/N2.vs.NFYB1.up.psp -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -nostatus -objfun classic -revcomp -markov_order 0

echo "4b"
#meme -oc ${psp_path}down/meme ${fasta_down} -psp ${psp_path}down/N2.vs.NFYB1.down.psp -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -nostatus -objfun classic -revcomp -markov_order 0

echo "5"
mkdir -p ${A}back_removal/up ${A}back_removal/down

#meme -oc ${A}back_removal/up ${fasta_up} -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -revcomp -objfun de -neg ${background_up}
meme -oc ${A}back_removal/down ${fasta_down} -dna -minw 5 -maxw 25 -nmotifs 20 -p 20 -revcomp -objfun de -neg ${background_down}

SIN

exit
```
