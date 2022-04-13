# parallel-examples

`parallel` allows you to replace messy loops with more elegant and concise code.

Here is a dummy sample processing script (e.g. `process_sample.sh`) that takes a
sample name as an input and does something:

    #!/usr/bin/bash

    # Get ID from the command line
    sample=$1

    # Announce that we're processing said sample
    echo "Processing data for '$sample'"

    # Add code here to process the sample

Here is how you can create a list of samples with replicates

    # Create an ids file
    parallel 'echo {1}_{2}' ::: WT EXP ::: 1 2 3 > ids.txt

That produces the following file:

    WT_1
    WT_2
    WT_3
    EXP_1
    EXP_2
    EXP_3

Here is how to use parallel to execute that processing script for each sample:

    # Run a script that processes each id
    cat ids.txt | parallel 'process_sample.sh {}'

And if you are using a scheduler, this works too (e.g. here using SLURM):

    # Run a script that processes each id
    cat ids.txt | parallel 'sbatch process_sample.sh {}'

An alternative using loops is a little messier and runs serially:

    for i in $(cat ids.txt); do sbatch process_sample.sh $i; done

## NOTES on parallel

By default, `parallel` runs as many threads as it has tasks to perform.
