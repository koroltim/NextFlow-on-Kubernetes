# Ignore failing process 

## Problem 

A task is expected to fail in a certain condition. You want to ignore the failure and continue the execution of the remaining tasks in the workflow. 

## Solution

Use the process https://www.nextflow.io/docs/latest/process.html#errorstrategy[directive] `errorStrategy 'ignore'` to ignore the error condition. 


## Code 

    process foo {
      errorStrategy 'ignore'
      script:
      '''
        echo This is going to fail!
        exit 1
      '''
    }  

    process bar {
      script:
      '''
      echo OK
      '''
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/ignore-failing-process.nf  -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
