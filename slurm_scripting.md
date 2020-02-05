# SLURM scripting

### Parallel processing ###
In general, there are 4 ways to effectively run programs in parallel (aka across multiple CPUs):
1. Using built-in parallelization of a program
2. Executing multiple programs in the background in parallel
3. Using a custom parallel driver program*
4. Using a workload manager (like SLURM)

### What is SLURM? ###
SLURM is a workload manager (SLURM = Simple Linux Utility for Resource Management) that can help you run programs in parallel. Slurm is powerful because (unlike \#1 and \#2 above) it allows you to run multiple jobs in parallel and set different processor and memory allocations. This is great if you want to run multiple programs that have different memory requirements. Another major benefit is that you can add jobs to the queue at any time and they will start as soon as computing resources become available. It also allows multiple users to share cores and submit jobs, and you can distribute jobs across multiple machines.

### SLURM scripting with sbatch ###
In order to fully take advantage of SLURM, you need to write a batch script. The basic structure of an sbatch script is: unix shell ID on the first line (e.g. #!/bin/bash), several lines of SLURM directives, then your executable code.

SLURM directives are denoted by #SBATCH, with one directive per line. This is where all the cool stuff is! You can use #SBATCH to tell SLURM how many cores you want to allocate to the job, how much memory, a maximum run time, where to send stderr, etc. One great directive is to have SLURM email you when the job starts/fails/completes. For example:



    #!/bin/bash
    #SBATCH -n 23                               #number of cores to use
    #SBATCH -N 1                                #number of nodes to use
    #SBATCH --mem 120000                        #memory to allocate
    #SBATCH -o cpro_processradtags_i2.out       #stdout
    #SBATCH -e cpro_processradtags_i2.err       #stderr
    #SBATCH -t 1-23:00                          #max allowed runtime
    #SBATCH --mail-type=ALL                     #email when job begins, fails, and ends
    #SBATCH --mail-user=<your email>            #address to which to send email
    #SBATCH --clusters=cbsumm09                 #name of cluster to submit job to

    <your executable code here>


There are many other directives you can give SLURM (see [this summary](https://slurm.schedmd.com/pdfs/summary.pdf) for more options).

### Basic SLURM commands (on the BioHPC or otherwise) ###
Once you've generated a batch script for SLURM, you'll generally want to follow these steps:

__List current clusters:__ Check to see if there are any clusters currently available.

    manage_slurm list

__Create a cluster:__ Most of the BioHPC machines run SLURM. To create a new cluster, you must have a current reservation on the machine(s). *Note that the machine name should be e.g. cbsumm09, not cbsumm09.tc.cornell.edu.*

    manage_slurm new <machinename1, machinename2>

__Submit a job to the cluster:__ Once you've created a cluster, you're ready to submit jobs. I like to run all jobs with nohup so that they continue even if I log out of terminal remotely.

    nohup sbatch <slurm_script.sh> &

__Check the status of your job:__ You can check the status of all jobs submitted to the cluster at any time. This will give you useful information, including job_id and runtime.

    squeue

__Cancel a job:__ Given the job id listed when you check the SLURM queue, you can easily cancel any job.

    scancel <job_id>

### Those are the basics! If you follow these steps, you'll be up and running on SLURM and computing efficiently in no time! ###

:tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada::tada:
