---
layout: post
title: Linking raw data files to the projects folder
---

First get the links of the raw data files in a text file using;

```
for f in $(ls *.gz) ; do echo "ln -s $(readlink -f ${f})"  ; done > /path/to/the/scripts_folder/scripts.AIqbal/links.sh
```

Then change directory to `/path/to/the/scripts_folder/scripts.AIqbal/` to edit the `links.sh` file and add the following 
at the top;

```
#!/bin/bash
mkdir -p ../raw_data
cd ../raw_data 
```
Rename the linked samples to more user understandable names by adding the new sample name after a space in the `links.sh` file after 
the link command. For example;

```
ln -s /beegfs/group_bit/data/raw_data/departments/Thomas_Langer/TL_Lasarzewski_RNAseq/bastet.ccg.uni-koeln.de/downloads/ylasarzewski_LA07/A006200063_115422_S32_L002_R1_001.fastq.gz HeLa_WT_REP1_READ_1.fastq.gz
```

Change permissions of the `links.sh` file to make it executable by;

```
chmod +x links.sh
```

Execute the file by;

```
./links.sh
```
