---
published: true
title: Merge FastQ files from different lanes
layout: post
---
Assuming the two lanes are `L001` and `L002`
```bash
#!/bin/bash

raw_data=/path/to/raw_data/

cd ${raw_data}

for f1 in $(ls *L001*); do
	for f2 in $(ls *L002*); do

		cat ${f1} ${f2} > ${raw_data}merged_files/${f1::(-16)}${f1:48}

	done
done

exit
```

Change `${f1::(-16)}${f1:48}` according to sample names.
