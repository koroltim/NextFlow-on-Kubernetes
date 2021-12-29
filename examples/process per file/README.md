# Process per file path  

## Problem 

You need to execute a task for each file that matches a glob pattern. 

## Solution

Use the https://www.nextflow.io/docs/latest/channel.html#frompath[Channel.fromPath] method to create a channel emitting all files matching the glob pattern. Then, use the channel as input of the process implementing your task. 


## Code 

    Channel.fromPath('reads/*_1.fq.gz').set{ samples_ch }

    process foo {
      input:
      file x from samples_ch
  
      script:
      """
      your_command --input $x
      """
    }



## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-per-file-path.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
