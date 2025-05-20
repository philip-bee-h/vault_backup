---
title: sge-example-script
created: 2025-01-24
type: resource
category: 
tags:
  - resource
  - tool-name
  - uiowa
related_docs:
---
This is an example script for my reference

```sh
#!/bin/bash
#$ -N Bert_Labeling                  # Job name
#$ -m beas                           # Send mail on begin, end, abort, and suspension
#$ -M alaa-albashayreh@uiowa.edu     # Email address for notifications 
#$ -pe smp 40                        # Changed to 40 cores as per queue config
#$ -q SYMPT                          # Queue name
#$ -l mem_total=256G                 # Total memory request
#$ -l gpu_l40s=true                  # Changed to match the new hardware
#$ -l ngpus=4

#$ -wd /Dedicated/ihdr_sgilbertsonwhite_202006264/Nahid/Paper1  # Working directory
#$ -e /Dedicated/ihdr_sgilbertsonwhite_202006264/Nahid/Paper1/err/  # Error log directory
#$ -o /Dedicated/ihdr_sgilbertsonwhite_202006264/Nahid/Paper1/out/  # Output log directory

# Load necessary modules
module load python/3.8.8_gcc-9.3.0
module load py-pip/20.2_gcc-9.3.0
module load cuda/11.7
module load cudnn/8.5.0_cuda-11.7

# Install accelerate
pip3 install "accelerate>=0.20.1"

# Execute the notebook
jupyter nbconvert --execute \
    --allow-errors \
    --ExecutePreprocessor.timeout=600 \
    --to notebook \
    "04-Apply Symptom-BERT.ipynb" \
    --output "04-Apply Symptom-BERT_out.ipynb"

# Send completion email
echo "Job finished" | mail -s "BERT job Notification" <user>@uiowa.edu
```