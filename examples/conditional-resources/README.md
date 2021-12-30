# Conditional resources definition 

## Problem 

A task in your workflow requires to use an amount of computing 
resources eg. memory that depends on the size or the name of one 
or more input files. 

## Solution 

Declare the resource requirements e.g. `memory`, `cpus`, etc.
in a dynamic manner using a closure. 

The closure computes the required amount of resources using the file 
attributes, such as `size`, etc., of the inputs declared in the process
definition.

## Code 

    Channel
        .fromPath('reads/*_1.fq.gz')
        .set { reads_ch }

    process foo {
        memory { reads.size() < 70.KB ? 1.GB : 5.GB }

        input:
        file reads from reads_ch 

        """
        your_command_here --in $reads
        """
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/conditional-resources.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html



Note: requires version 0.32.0 or later.
