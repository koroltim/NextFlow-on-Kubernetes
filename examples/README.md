# General guide to running examples

## Install NextFlow
First you have to install nextflow by running this command:

     curl -s https://get.nextflow.io | bash
     
## Clone the examples repository

Clone NextFlow examples repository by running this command:

    git clone https://github.com/nextflow-io/patterns.git
    
## Create nextflow.config file

After you've cloned the repository,you should cd to the directory where you've installed NextFlow and run this command:

    cd .nextflow/assets/nextflow-io/patterns
    
At last you should copy the nextflow.config from the project's files to the directory and don't forget to change the namespace value in k8s section to represent your namespace which is usually in format surname-ns.

## Run the example you want

Now you're ready to cd back to the directory you have installed NextFlow into and run whatever example you want by following their individual guidelines.
