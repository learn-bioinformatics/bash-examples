# bash-examples

## parallel 

`parallel` allows you to easily run the same script on different samples.

Here is a dummy sample processing script that takes a sample name as an input and does something:

    #!/usr/bin/bash

    # Get ID from the command line
    sample=$1

    # Announce that we're processing said sample
    echo "Processing data for '$sample'"

    # Add code here to process the sample

Here is a dummy script showing how to create a list of samples with replicates 
    # Create an ids file
    parallel 'echo {1}_{2}' ::: WT EXP ::: 1 2 3 > ids.txt

That produces the following file:

    WT_1
    WT_2
    WT_3
    EXP_1
    EXP_2
    EXP_3

Here is how to use parallel to excute that processing script for each sample:

    # Run a script that processes each id
    cat ids.txt | parallel 'script.sh {}'

And if you are using a scheduler, this works too (e.g. here using SLURM):

    # Run a script that processes each id
    cat ids.txt | parallel 'sbatch script.sh {}'
