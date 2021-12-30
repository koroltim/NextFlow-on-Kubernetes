# Channel duplication   

## Problem 

You need to you use the same channel as input in two or more processes.

## Solution

Use the https://www.nextflow.io/docs/latest/operator.html#into[into] operator to create two (or more) copies of the source channel. Then, use the new channels as input for the processes. 

## Code 

        Channel
            .fromPath('prots/*_?.fa')
            .into { prot1_ch; prot2_ch }

        process foo {
          input: file x from prot1_ch
          script: 
          """
            echo your_command --input $x
          """
        }    

        process bar {
          input: file x from prot2_ch
          script: 
          """
            your_command --input $x
          """
        }    

## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/channel-duplication.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
