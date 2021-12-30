# Optional input 

## Problem 

One or more processes have an optional input file. 

## Solution 

Use a special file name to mark the absence of the file parameter. 

## Code 

    params.inputs = 'prots/*{1,2,3}.fa'
    params.filter = 'NO_FILE'

    prots_ch = Channel.fromPath(params.inputs)
    opt_file = file(params.filter)

    process foo {
      input:
      file seq from prots_ch
      file opt from opt_file 

      script:
      def filter = opt.name != 'NO_FILE' ? "--filter $opt" : ''
      """
      your_commad --input $seq $filter
      """
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/optional-input.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html

Run the same script providing an optional file input:

    nextflow kuberun patterns/optional-input.nf --filter foo.txt -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt
