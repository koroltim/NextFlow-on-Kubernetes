# Conditional process executions 

## Problem 

Two different tasks need to be executed in a mutually exclusive manner, 
then a third task should post-process the results of the previous execution.

## Solution

Use a https://www.nextflow.io/docs/latest/process.html#when[when] statement to conditionally 
execute two different processes. Each process declares its own output channel.

Then use the https://www.nextflow.io/docs/latest/operator.html#mix[mix] operator to create 
a new channel that will emit the outputs produced by the two processes and use it as the input
for the third process.

## Code 

    params.flag = false 

    process foo {
      output: 
      file 'x.txt' into foo_ch
      when:
      !params.flag

      script:
      '''
      echo foo > x.txt
      '''
    }

    process bar {
      output: 
      file 'x.txt' into bar_ch
      when:
      params.flag

      script:
      '''
      echo bar > x.txt
      '''
    }

    process omega {
      echo true
      input:
      file x from foo_ch.mix(bar_ch)

      script:
      """
      cat $x 
      """
    }


## Run it

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/conditional-process.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html

The processes `foo` and `omega` are executed. Run the same command 
with the `--flag` command line option. 

    nextflow kuberun patterns/conditional-process.nf --flag -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt 

This time the processes `bar` and `omega` are executed.


## Alternative solution

Conditionally create the input channels normally (with data) or as 
https://www.nextflow.io/docs/latest/channel.html#empty[empty] channels. 
The process consuming the individual input channels will only execute if 
the channel is populated. Each process still declares its own output channel.

Then use the https://www.nextflow.io/docs/latest/operator.html#mix[mix] operator to create 
a new channel that will emit the outputs produced by the two processes and use it as the input
for the third process.

## Code 

    params.flag = false

    (foo_inch, bar_inch) = ( params.flag
                         ? [ Channel.empty(), Channel.from(1,2,3) ]
                         : [ Channel.from(4,5,6), Channel.empty() ] )   

    process foo {

      input:
      val(f) from foo_inch

      output:
      file 'x.txt' into foo_ch

      script:
      """
      echo $f > x.txt
      """
    }

    process bar {
      input:
      val(b) from bar_inch

      output:
      file 'x.txt' into bar_ch

      script:
      """
      echo $b > x.txt
      """
    }

    process omega {
      echo true
      input:
      file x from foo_ch.mix(bar_ch)

      script:
      """
      cat $x
      """
    }


## Run it 

First you should follow the instructions mentioned in examples folder of this git repo.

After that you can use the the following command to execute the example:

    nextflow kuberun patterns/conditional-process2.nf -pod-image 'cerit.io/nextflow:21.09.1' -v PVC:/mnt

Where you should replace PVC with your actual PVC, you've created before.
You can create your PVC by following this guideline https://cerit-sc.github.io/kube-docs/docs/pvc.html
