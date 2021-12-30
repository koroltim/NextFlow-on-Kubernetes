# Process per file range 

## Problem 

You need to execute a task over two or more series of files having a common index range. 

## Solution 

Use a the https://www.nextflow.io/docs/latest/channel.html#from[from] method 
define the range over which repeat the task execution, 
then chain it with a https://www.nextflow.io/docs/latest/operator.html#map[map] operator 
to associate the each index the corresponding input files. Finally use the resulting channel as input for the process.

## Code 

    Channel
      .from(1..23)
      .map { chr -> tuple("sample$chr", file("/some/path/foo.${chr}.indels.vcf"), file("/other/path/foo.snvs.${chr}.vcf")) }
      .set { pairs_ch }


    process foo {
      tag "$sampleId"

      input: 
      set sampleId, file(indels), file(snps) from pairs_ch

      """
      echo foo_command --this $indels --that $snps
      """
    } 


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-per-file-range.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html

