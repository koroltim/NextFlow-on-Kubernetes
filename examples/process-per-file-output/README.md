# Process per file output 

## Problem 

A task in your workflow produces two or more files at time. A downstream task needs to process each
of these files independently.

## Solution

Use the https://www.nextflow.io/docs/latest/operator.html#flatten[flatten] operator to 
transform the outputs of the upstream process to a channel emitting each file separately. 
Then use this channel as input for the downstream process. 


## Code 

    process foo {
      output:
      file '*.txt' into foo_ch 
      script:
      '''
      echo Hello there! > file1.txt
      echo What a beautiful day > file2.txt
      echo I wish you are having fun1 > file3.txt 
      ''' 
    }

    process bar {
      input: 
      file x from foo_ch.flatten()
      script:
      """
      cat $x
      """
    }


## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-per-file-output.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html

