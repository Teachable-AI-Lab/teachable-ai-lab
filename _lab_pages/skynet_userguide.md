---
layout: post
title:  Skynet Userguide 
date:   2023-6-15
permalink: /skynet-userguide/
author: Christopher J. MacLellan
comments: false
---

SkyNet is a GPU-intensive cluster shared by research labs at GaTech. It pools
resources (money, time, attention) to create and maintain the best compute
cluster in academia for modern AI/ML research. You can learn more about it
here: <http://optimus.cc.gatech.edu/wiki/skynet>. Skynet manages jobs using the
slurm system, and you will need to use slurm to interface with it. This guide
explains the basics of how to use skynet and slurm.

* TOC
{:toc}

# What is slurm?
It is an "an open source, fault-tolerant, and highly scalable cluster
management and job scheduling system for large and small Linux clusters". You
can learn more about it here:
<https://en.wikipedia.org/wiki/Slurm_Workload_Manager>

At its core, slurm provides a way to request access to a certain compute
resources for a certain period of time. It aims to allocate these resources
from the compute cluster in a efficient and fair fashion. 

To get access to skynet, our lab has contributed compute and storage nodes to
the cluster. In return, we can use the resources we contributed as well as any
additional idle / unused compute resources across the cluster. Depending on the
current load on the cluster, this should make it possible to get more compute
out than we contributed for burst activities. It also produces more efficient
utilization of the cluster's compute resources.

# How do I get access and get started? 
First, follow the steps here:
<http://optimus.cc.gatech.edu/wiki/skynet#how-to-get-access>

Once you do this, then you can log into a skynet head node using ssh; e.g.,
`ssh sky1.cc.gatech.edu`.

After logging in, you'll see that your home directory is quite small (30G), you
should set up a space in our lab's fileserver to work with.

Navigate to `/srv/tail-lab/` and create a folder with your username (e.g., for
me I created `/srv/tail-lab/cmaclellan3`). 

You should put all the files you work with here, so you do not fill up the hard
dive space available in your home directory and so that you can access these
files from any node in the skynet cluster. To make things easier, I created a
system link in my home directory that points to this folder: `ln -s
/srv/tail-lab/flash10/cmaclellan3/ ~/nostalgia`. This means that there is a
folder in my home directory called nostalgia that links to this location.

Once this is set up, you should pretty much do EVERYTHING using slurm. For
example, you should slurm to do things like unzipping tarballs, running a data
preprocessing script (even single core one), etc. Typically, you will use an
interactive slurm session for this, which we will cover next.

# How do I use slurm?

## Interactive jobs
The place to start is probably running an interactive job. This basically will
allocate you a terminal with the resources that you requested.

Once you are logged into a skynet head node, you should launch `tmux` so you
can come back if you get disconnected. Afterwards, you can can launch an
interactive slurm shell with the following command: `srun -p debug -J "test" --pty bash`

This runs a slurm job using the "debug" priority level, which is the highest.
It also has a max runtime of 4 hours, so your session will get killed if it
exceeds that amount of time. The -J option provides a name for the slurm job,
in this case, we called it "test" but you can name it other things. The `--pty`
option requests a pseudo terminal for your interactive session. Lastly the
`bash` command will be executed within the allocated resources, this starts
your interactive bash shell in the terminal. 

It is also possible to request specific resources using standard slurm commands. For example:
* `--gres=gpu:1` will request 1 GPUs for your use
* `--constraint=a40 --gpus-per-node=2` will request 2x A40 GPUs
* `--cpus-per-task <num_cpus>` or `-c <num_cpus>` (for short) will specify the number of cpus you want
* `--mem=4G` will request 4G of system memory
* `-w consu` to request the specific compute node consu

There are a couple of types of GPUs available on skynet:
* Titan XP: `--constraint=titan_x`
* RTX 2080 Ti: `--constraint=2080_ti`
* Quadro RTX 6000: `--constraint=rtx_6000`
* A40: `--constraint=a40`

Note, the constraints support disjunctions, so you can specify multiple
acceptable options; e.g., to get a titan xp or a 2080 ti you can put:
`--constraint="titan_x|2080_ti"` (note you need to put the constraints in
quotes here).

## Single executable
While interactive jobs are great for development, the are limited in runtime.
Jobs expected to take longer than the debug queue allows must be submitted to
the non-interactive short or long queues as independent processes or as batch
scripts. Unlike interactive jobs, jobs must be tied to a specific executable or
script and resources are released upon termination of that process. 

Suppose I have a job that I want to run on a single gpu (don't care which kind)
and I expect it to run for no more than 8 hours. I could directly submit this
process to the queue using srun: `srun -p short -t 08:00:00 --gres=gpu:1 /srv/tail-lab/flash10/cmaclellan3/nbody -numbodies=200000 -device=0 -benchmark |& tee task.log`
* `srun -p short -t 08:00:00 –gres=gpu:1` specifies that the job is for the
  short queue (`-p short`), has a self-imposed max runtime of 8 hours 
  (`-t 08:00:00`), and requires 1 GPU (`–gres=gpu:1`).
* `/srv/tail-lab/flash10/cmaclellan3/nbody -numbodies=200000 -device=0 -benchmark`
  is the process I want to run with its arguments.
* `|& tee task.logfile` is used to print a copy of all console output to
  `task.logfile` and is great for keeping track of different experiments.

After issuing this command, the shell will wait for allocation before executing
the process on the reserved node. Like with interactive jobs, the process is
tied to the executing shell so it is advised to run any srun commands from
within a screen.

## Batch Jobs
If you prefer to have some jobs running in the background rather than have a
number of screens, you should consider submitting batch jobs to the queue.

To submit a batch job, you have to create a script specifying your job and its
options. An example script myjob.sh is shown below:

```
#!/bin/sh -l
#SBATCH -p short
#SBATCH --gres=gpu:2
#SBATCH --constraint=a40
#SBATCH -J my_short_job
#SBATCH -o my_short_job.log
hostname
echo $CUDA_AVAILABLE_DEVICES
srun /srv/tail-lab/cmaclellan3/nbody -numbodies=200000 -device=0,1 -benchmark
```

The `#SBATCH` commands specify which queue (`-p short`), the number and type of
requested GPUs (`--gres=gpu:2`), the type of GPU (`--constraint=a40`), a name
for my job when it appears on the queue (`-J my_short_job`), and an output file
where console output will be redirected (`-o my_short_job.log`). Multiple
processes can be executed in sequence in the batch script. To submit the job
script, simply run:

```
cmaclellan3@sky1:~$ sbatch myjob.sh
Submitted batch job 150
cmaclellan3@sky1:~$
```

and the ID of your job will be repeated before control is returned to the
shell. If I check the queue while my job is running, I'll see the name of my
job, its node, and its status.

```
cmaclellan3@sky1:~$ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            590645     short     my_short_job cmaclell  R       0:08      1 alexa
```

After it has finished (or any time before) I can check the output file to see
its progress / result:

```
cmaclellan3@sky1:~$ cat my_short_job.log
consu.cc.gatech.edu
GPUS: 0,1
Run "nbody -benchmark [-numbodies=<numBodies>]" to measure performance.
.
.
.
200192 bodies, total time for 10 iterations: 3255.234 ms
= 123.115 billion interactions per second
= 2462.302 single-precision GFLOP/s at 20 flops per interaction
```

Note: Despite which physical GPU you are actually allocated, the ID's for
`CUDA_VISIBLE_DEVICES` will be set starting from 0 for all jobs (batch or
otherwise). This actually makes setting device IDs for programs a bit easier!

## Slurm Priority Levels 
SLURM is configured to have three queues which share all of the managed CPUs
and GPUs:

| Queue          | Max Runtime | Priority | Allows Interactive Jobs | Notes                                                                 |
|----------------|-------------|----------|-------------------------|-----------------------------------------------------------------------|
| debug (default)| 4 hours     | 2        | Yes                     | Only up to 5 GPUs at once in debug                                    |
| short          | 2 days      | 1        | No                      |                                                                       |
| long           | 1 week      | 0        | No                      |                                                                       |
| user-overcap   | 2 days      | ?        | Yes                     | Jobs will be preemptable, lets you use GPUs, but doesn't lock others out of using them. |
| overcap        | 2 days      | ?        |                         | Allows you to exceed a given lab's GPU cap but is preemptable.         |
|                |             |          |                         | NOTE: To use this you must run in the overcap slurm account by adding `-A overcap` to your args. |

Note, that slurm allows you to submit to multiple partitions at once, i.e.
`short,user-overcap` will have a job first fill up your user GPU cap on short
and then will spill over into `user-overcap` as needed.

Each queue has different settings to accommodate the different ways we use the
GPUs in our cluster. These differences are defined by:
* **Max Runtime** - At the end of the maximum runtime, jobs will be sent
  SIGTERM. After a grace period (5 minutes), the process will be sent a SIGKILL
  and be terminated. If your code is doing regular checkpointing then there
  should be an upper bound to how much progress is lost; however, a better choice
  is to set up a signal handler to make the SIGTERM trigger a checkpoint.
* **Priority** - This is a non-preemptive priority meaning if a GPU becomes
  available, priority will be given to jobs with higher priority numbers. This
  lets us run jobs from the shorter queues before longer jobs when resources are
  available.
* **Interactive Jobs** - So no one accidentally keeps a GPU for longer than a
  job takes, we are limiting interactive sessions to the debug queue.
  Non-interactive jobs are bound to specific processes and release their
  resources when the process terminates.

## Checking the queues
You can use the command `squeue` to check the status of your slurm jobs on the cluster.

This will output something like:

```
cmaclellan3@sky1:~$ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            590647     debug     bash bwilson6  R       4:51      1 spot
            590642     debug     bash rramrakh  R      13:23      1 megabot
            590617     debug    inter myang415  R      51:14      1 alexa
            590584     debug     test   okara7  R    2:54:31      1 alexa
            590272      long  python3  abeedu3  R   20:08:29      1 nestor
            590337      long  python3  abeedu3  R   19:27:27      1 megazord
```

You can use this to find your job and its job id.

## Canceling a job
Canceling a running interactive or executable job is easy, just kill the
process from the shell that is running it. For batch jobs, running scancel
jobid terminates the associated job in the same way as if it had run out of
time (i.e. terminated after a 5 minute grace window after issuing a SIGTERM).
To get your job ID, just check the queue using the commands from the section
above this one.

# Frequent Issues
Please see here for a list of frequent issues:
<http://optimus.cc.gatech.edu/wiki/skynet#expectations-and-advice_frequently-encountered-issues>

# Setting up pyenv

Many people use conda, but I much prefer pyenv.

Typically, I install pyenv using
[pyenv-installer](https://github.com/pyenv/pyenv-installer). This lets me
install with a single command: `curl https://pyenv.run | bash`.

Once I installed pyenv, I moved the `~/.pyenv` folder to the lab fileserver at
`/srv/tail-lab/flash10/cmaclellan3/.pyenv`, which way my virtual machines are
not taking up space on my limited user space.

I then updated my `~/.bashrc` file to contain the following pyenv setup:
```
export PYENV_ROOT="/srv/tail-lab/flash10/cmaclellan3/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

You can see how I set up these paths to point to my new `.pyenv` location on the fileserver.

One of the challenges with installing python versions using pyenv is that you
need some basic dependencies installed on the host machine. In particular, we
need openssl-1.1.1 for newer versions of python. I've compiled openssl-1.1.1
and stored it in `/srv/tail-lab/flash10/shared_packages/openssl`. You can use
the libraries here to install newer versions of python with pyenv using the
following.

You can install your python 3.7+ using:
```
CC=/srv/tail-lab/flash10/shared_packages/gcc-13/bin/gcc \
CXX=/srv/tail-lab/flash10/shared_packages/gcc-13/bin/g++ \
LDFLAGS="-Wl,-rpath,/srv/tail-lab/flash10/shared_packages/openssl-1.1.1u/lib -Wl,-rpath,/srv/tail-lab/flash10/shared_packages/libffi-3.4.4/lib -Wl,-rpath,/srv/tail-lab/flash10/shared_packages/gcc-13/lib64 -Wl,-rpath,/srv/tail-lab/flash10/shared_packages/sqlite-autoconf-3420000/lib"
PYTHON_CFLAGS="-march=native" \
CONFIGURE_OPTS="--enable-optimizations --with-lto --with-openssl=/srv/tail-lab/flash10/shared_packages/openssl-1.1.1u" \
pyenv install -v 3.11.4
```

Note, this uses the precompiled gcc and openssl, libffi, and sqlite3-dev
libraries. It also uses the `march=native` and `--enable-optimizations
--with-lto` flags, which make the build take significantly longer, but
increases the speed of python around 30%.

To get a version of python that has full c++ build support (e.g., that
supported c++17 and c++20 standards), I had to manually build gcc. Let me know
if you're having build issues and maybe we can sort it out. 
