# Process per file chunk  

## Problem 

You need to split one or more input files into chunks and execute a task for each of them.

## Solution

Use the the https://www.nextflow.io/docs/latest/operator.html#splittext[splitText] operator to split a file in chunks of a given size. Then use the resulting channel as input for the process implementing your task. 

Caveat: By default chunks are kept in memory. When splitting big files specify the parameter `file: true` to save the chunks into files. See the https://www.nextflow.io/docs/latest/operator.html#splittext[documentation] for details.

Splitter for specific file formats are available, eg https://www.nextflow.io/docs/latest/operator.html#splitfasta[splitFasta] and https://www.nextflow.io/docs/latest/operator.html#splitfastq[splitFastq].
 

## Code 

    Channel
        .fromPath('poem.txt')
        .splitText(by: 5)
        .set{ chunks_ch }

    process foo {
      echo true
      input: 
      file x from chunks_ch

      script:
      """
      rev $x | rev
      """
    } 


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/process-per-file-chunk.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html

