# Skip process execution 

## Problem 

You have two sequential tasks in your workflow. When an optional flag is specified 
the first task should not be executed and its input(s) is processed by the second task.

## Solution

Use an empty channel, created in a conditional expression, to skip the 
first process execution when an optional parameter is specified. 

Then, define the second process input as a https://www.nextflow.io/docs/latest/operator.html#mix[mix] 
of the first process output (when executed) and the input channel.

## Code 

    params.skip = false
    params.input = "$baseDir/sample.fq.gz" 

    Channel.fromPath(params.input).set{ input_ch }

    (foo_ch, bar_ch) = ( params.skip 
                     ? [Channel.empty(), input_ch] 
                     : [input_ch, Channel.empty()] ) 

    process foo {
      input:
      file x from foo_ch

      output:
      file('*.fastq') into optional_ch

      script:
      """
      < $x zcat > ${x.simpleName}.fastq
      """
    }

    process bar {
      echo true
      input: 
      file x from bar_ch.mix(optional_ch)
      """
      echo your_command --input $x
      """
    }


## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/skip-process-execution.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html


The processes `foo` and `bar` are executed. Run the same command 
with the `--skip` command line option. 

    nextflow kuberun patterns/skip-process-execution.nf --skip -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt
    
This time only processes `bar` is executed.
