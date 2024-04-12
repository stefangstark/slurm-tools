# slurm-tools
Some QOL tools I wrote for interacting with SLURM.
- during my phd we had a secure compute enviornment with limited access to internet, experiment management tools
- these scripts proved to be helpful in simplifying, managing, & monitoring my cluster interaction

## Job Submission with `sprep`
`sprep` helps group and submit commands as a SLURM job group

Given a list of commands, `sprep` writes them to a text file and then submits a SLURM command to run commands from this text file
- basically a CLI version of this [tutorial](https://scicomp.ethz.ch/wiki/Job_arrays#Using_a_.22commands.22_file)

I typically ran it using this format:

```./list-commands | sprep -J JOB_NAME -N TOTAL_JOBS | sbatch ADDL_SBATCH_OPTS```

- `list-commands` is usually output from a script, but can also be formed from a `find` command
- SLURM needs the job's name in order to specify the array
  - so generally best to use `-J` in `sprep`, not in `sbatch`
- you can also use `... | sprep -I CMDS_PER_SPLIT | sbatch ...` to group many short commands into a single SLURM job
  - and now you dont need to do division 🙂

I liked this set up as it was quite lightweight & flexible
- the first pipe can be modified (eg `grep`) to pick out specific commands to run or re-run
- the second to modify throughput
- the third to modify resources required

## Job Monitoring with `sjobs`
`sjobs` prints a table of running job groups & their statuses
- similar to the LSF [`bjobs`](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=bjobs-options)
- defaults to running jobs, but can specify a start time
  - eg show me all jobs that were submitted last friday afternoon
