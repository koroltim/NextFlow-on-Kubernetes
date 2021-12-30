# Rename process outputs 

## Problem 

You need to store the outputs of a process to a directory 
giving files a name of your choice.

## Solution 

The https://www.nextflow.io/docs/latest/process.html#publishdir[publishDir] 
allows you to store the process outputs in a directory of your choice. 

Specify the `saveAs` parameter to give each file a name of your choice proving 
a custom rule as a https://www.nextflow.io/docs/latest/script.html#closures[closure]. 

## Code 

    process foo {
     publishDir 'results', saveAs: { filename -> "foo_$filename" }

     output: 
     file '*.txt'

     '''
     touch this.txt
     touch that.txt
     '''
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/publish-rename-outputs.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html



## Save outputs in a sub-directory

The same pattern can be used to store specific files in separate directories 
depending the actual name. 

     process foo {
      publishDir 'results', saveAs: { filename -> filename.endsWith(".zip") ? "zips/$filename" : filename }

      output: 
      file '*'

      '''
      touch this.txt
      touch that.zip
      '''
     }


TIP: Relative paths are resolved against the `publishDir` store path. Use an absolute path 
to store files in a directory outside the `publishDir` store path. 


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/publish-rename-outputs-subdirs.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
