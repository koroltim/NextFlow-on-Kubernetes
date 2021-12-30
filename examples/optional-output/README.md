# Optional output  

## Problem 

A task in your workflow is expected to not create an output file in some circumstances. 

## Solution

Declare such output as an `optional` file. 

## Code 

    process foo {
      output: 
      file 'foo.txt' optional true into foo_ch 

      script:
      '''
      your_command 
      '''
    }


## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/optional-output.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
