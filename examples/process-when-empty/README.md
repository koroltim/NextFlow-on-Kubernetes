# Execute when empty 

## Problem 

You need to execute a process if a channel is empty. 

## Solution 

Use the https://www.nextflow.io/docs/latest/operator.html#ifempty[ifEmpty] operator to emit 
a _marker_ value to trigger the execution of the process. 

## Example 

    process foo {
      input:
      val x from ch.ifEmpty { 'EMPTY' } 
      when:
      x == 'EMPTY'

      script:
      '''
      your_command
      ''' 
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-when-empty.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html


Use the following command to provide the same script
some input files, that prevents the process to be executed: 

    nextflow kuberun patterns/process-when-empty.nf --inputs ../data/prots/\* -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt
