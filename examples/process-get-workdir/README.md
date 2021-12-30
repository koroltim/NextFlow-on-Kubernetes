# Get process work directory

## Problem 

A tool need the explicit path of the current task work directory.

## Solution 

Use the `$PWD` Bash variable or the `pwd` command to retrieve the task working directory path. 

Note: Make sure use to escape the `$` variable placeholder 
when the command script is enclosed in double quote characters.


## Code 

    process foo {
      echo true
      script:
      """
      echo foo task path: \$PWD
      """ 
    }

    process bar {
      echo true
      script:
      '''
      echo bar task path: $PWD
      ''' 
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-get-workdir.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html


Use the following command to provide the same script
some input files, that prevents the process to be executed: 

    nextflow kuberun patterns/process-get-workdir.nf --inputs ../data/prots/\* -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt
