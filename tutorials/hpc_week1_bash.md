---
layout: default
title: Week 1: Bash and HPC Basics
permalink: /tutorials/hpc_week1_bash/
---

# Week 1: Bash and HPC Basics for Bioinformatics

Modern bioinformatics analyses almost always run on **high-performance computing (HPC) clusters** rather than personal laptops. These systems allow analyses to scale across large datasets by distributing work across many CPUs and large memory nodes.

However, many analyses fail not because of complex software and algorithms, but because of **poor organization and poor workflow management**. Learning a few basic HPC and bash principles early dramatically improves reproducibility and efficiency.

This tutorial introduces:

- HPC architecture
- cluster storage systems
- basic bash navigation
- job schedulers
- submitting your first job

---

# Organizing Work and Documentation

Keeping track of analyses is critical for reproducibility.

A useful habit is maintaining a **Markdown notebook** describing:

- what you ran
- why you ran it
- commands used
- results produced

Markdown is a lightweight markup language that renders cleanly into formatted text.

Example editors:

- Typora (desktop)
- https://stackedit.io (browser-based)
- VSCode with Markdown preview

A Markdown cheat-sheet can be found here:

https://www.markdownguide.org/cheat-sheet/

---

# HPC Architecture

Most HPC systems follow the same structure.

![hpc_architecture](/images/hpc_overview.png)

## Login Nodes

When connecting to a cluster using:

```bash
ssh username@ceres.scinet.usda.gov
```

you typically arrive on a **login node**.

Login nodes are used for:

- editing files
- organizing directories
- submitting jobs
- transferring data

They are **not intended for running heavy analyses**.

## Compute Nodes

Actual analyses run on **compute nodes**, which provide:

- CPUs / GPUs
- RAM

Access to compute nodes is controlled by a **job scheduler**.

------

# Storage on HPC Systems

Most clusters provide several types of storage.

| Location        | Purpose                              | Typical Size           | Persistence  |
| --------------- | ------------------------------------ | ---------------------- | ------------ |
| `$HOME`         | personal files and software installs | small (50 Gb on ceres) | permanent    |
| project storage | shared group data                    | large                  | permanent    |
| scratch storage | temporary working files              | very large             | auto-deleted |

Examples of paths on ceres:

```
/home/$USER
/project/$PROJECT_ID
/90daydata/$PROJECT_ID
```

Find more details for ceres [here](https://scinet.usda.gov/guides/resources/ceres).

## General Best Practices

Install software in:

```
$HOME
```

Run analyses in your scratch directory:

```
/90daydata/$PROJECT_ID
```

Store final outputs in your project directory:

```
/project/$PROJECT_ID
```

------

# Bash Essentials

Most HPC systems use **bash**, a command-line shell.

Below are essential commands used constantly in bioinformatics.

## Print Current Directory

```
pwd
```

## List Files

```
ls
```

Useful variants:

```
ls -lh
ls -lhtr # list in list format, human readable, reverse time order (recent files at the bottom)
ls -a
```

`-a` shows hidden files.

------

## Move Between Directories

```
cd directory_name
```

Return to home directory:

```
cd
```

or

```
cd ~/
```

------

## Create Directories

```
mkdir results
```

Create nested directories:

```
mkdir -p project/data/raw
```

------

## Copy Files

```
cp file1 copy_of_file1
```

Copy directories just add recursive `-r`:

```
cp -r directory new_directory
```

------

## Move or Rename Files

```
mv file1.txt new_name.txt
```

------

## Remove Files

```
rm file
```

Remove directories:

```
rm -r directory
```

Careful! Adding recursive deletion with `-r` will delete all folders and files within that specified directory. 

------

# Hidden Files and `.bashrc`

Files beginning with a `.` are hidden.

View them with:

```
ls -a
```

Nearly all clusters have a set-up file in your home directory:

```
~/.bashrc
```

This file stores:

- environment variables
- aliases
- PATH settings

Sometimes it's called a `~/.bash_profile`, it depends on your HPC. 

------

# Aliases

Aliases create shortcuts for frequently used commands.

Example:

```
alias checkjobs="squeue --me"
```

Add this line to your `~/.bashrc`.

You can add this either by editing your `~/.bashrc` with `nano` or `vim`, or using echo:

```
echo 'alias checkjobs="squeue --me"' >> ~/.bashrc
```

The `echo` command will add that line to the end of your `~/.bashrc`, which you can check with the `tail` command, just checking the last line `-n 1`:

```
tail ~/.bashrc -n 1
alias checkjobs="squeue --me" 
```

Reload the shell with:

```
source ~/.bashrc
```

Now you can run:

```
checkjobs
```

instead of typing the full command.

------

# Symlinks (Directory or file Shortcuts)

Long paths become tedious.

Example path:

```
/project/$PROJECT_ID/genomes/assembly/run1
```

A **symbolic link** creates a shortcut. 

As long as you're in your home directory (type `cd`)

```
ln -s /project/$PROJECT_ID/genomes/assembly assembly
```

Now you can navigate to that folder directly using:

```
cd assembly
```

Remove a symlink:

```
unlink assembly
```

This removes the shortcut **without deleting the original directory**.

------

# Job Schedulers

Clusters allocate computational resources through a **job scheduler**.

Common schedulers include:

- SLURM
- PBS

Schedulers manage:

- CPU allocation
- memory usage
- runtime limits
- job queues

This tutorial uses **SLURM**, which is widely used in research clusters.

------

# Writing a SLURM Job Script

Create a file:

```
test.sh
```

Example script:

```
#!/bin/bash

#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G

echo "Hello from the compute node"
echo "Job ID: $SLURM_JOB_ID"

sleep 30
```

On ceres, you will need to add ceres-specific annotations like `#SBATCH --account`, which you can find more details [here](https://scinet.usda.gov/guides/use/slurm), including [a SLURM header generation script](https://scinet.usda.gov/support/ceres-job-script). 

To run on ceres and "bill" the hours to my project, I would need to tell SLURM which "partition" to use, and which account to bill:

```
#!/bin/bash

#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --partition=ceres
#SBATCH --account=$PROJECT_ID
```

## Shebang

The first line of a script:

```
#!/bin/bash
```

This tells the system which interpreter to use.

Examples for other languages:

```
#!/usr/bin/env python3
#!/usr/bin/env Rscript
```

------

## Resource Requests

Maximum runtime:

```
#SBATCH --time=01:00:00
```

Number of CPUs:

```
#SBATCH --cpus-per-task=1
```

Requested memory:

```
#SBATCH --mem=4G
```

Some clusters like ceres also require:

```
#SBATCH --account=my_lab
#SBATCH --partition=ceres
```

Check your cluster [docs](https://scinet.usda.gov/guides/use/slurm). 

------

# Submitting Jobs

Submit a job with:

```
sbatch test.sh
```

The scheduler returns a **job ID**.

------

# Checking Job Status

View running jobs:

```
squeue --me
```

Example output:

```
JOBID PARTITION NAME USER ST TIME NODES NODELIST(REASON)
20048101 compute test.sh user R 0:03 1 node14
```

Common job states:

| Code | Meaning    |
| ---- | ---------- |
| PD   | pending    |
| R    | running    |
| CG   | completing |
| CD   | completed  |

------

# Job Output

SLURM writes output to:

```
slurm-JOBID.out
```

Example:

```
cat slurm-20048101.out
```

Output:

```
Hello from the compute node
Job ID: 20048101
```

------

# Monitoring Resource Usage

After jobs complete it is good practice to check resource usage.

Some clusters like ceres allow this easily with: 

```
seff JOBID
```

If you're feeling fancy, check out [reportseff](https://github.com/troycomi/reportseff), which allows you to type `reportseff slurm*`, returning stats for all `slurm*.out` files in your current directory!

Others clusters might use:

```
sacct -j JOBID
```

Typical output includes:

- CPU efficiency
- memory usage
- runtime

Understanding this helps optimize future jobs.

------

# Summary

Things to remember: 

1. HPC systems separate **login nodes** from **compute nodes**.
2. Analyses run through a **job scheduler**.
3. Good **file organization** is essential for reproducibility.
4. Bash navigation commands are used constantly.
5. Job scripts specify **time, CPUs, and memory requirements**.
