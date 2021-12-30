# Process all outputs altogether  

## Problem 

You need to process all the outputs of an upstream task altogether. 

## Solution

Use the https://www.nextflow.io/docs/latest/operator.html#collect[collect] operator to gather 
all the outputs produced by the upstream task and emit them as a sole output. 
Then use the resulting channel as input input for the process.

## Code 

    Channel.fromPath('reads/*_1.fq.gz').set { samples_ch }

    process foo {
      input:
      file x from samples_ch
      output:
      file 'file.fq' into unzipped_ch
      script:
      """
      < $x zcat > file.fq
      """
    }

    process bar {
      echo true   
      input:
      file '*.fq' from unzipped_ch.collect()
      """
      cat *.fq
      """
    }


## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-collect.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
