---
layout: single
title:  "Memory requests in HPC systems"
date:   2025-01-03
category:
    HPC
---

I recently had to move a lot of individual scripts, including their SLURM submission scripts, into a snakemake workflow. In doing so I found that when snakemake made memory requests it was doing things slightly differently than me. Today I'd like to write down a few notes on getting the most memory for your scripts when submitting jobs on a SLURM cluster, because it's often a bottleneck in genomics. 

You have a few general options when submitting jobs. The ones I'll focus on here are:

- mem - total memory requested
- mem-per-cpu - as it says, memory per cpu
- tasks - the number of independent processes. Usually set to 1
- cpus-per-tasks - number of CPUs, set as >1 if your code can use multiple CPUs, otherwise 1 if single threaded

You'll also see your partitions and some abreviated node names in the terminal by typing `sinfo`. This won't tell you memory available per node by default. To get more detail you want `sinfo -Nl`, for nodes per partition, and the more important `scontrol show nodes` to give you all node information. Under this you will be able to see RealMemory, which will tell you what's available in Mb. Note that you can also see mem of past jobs with `sacct -o JobID,JobName,Partition,MaxRSS,ReqMem,AllocTRES`, where MaxRSS gives you what you want, but here I'm focussing on requesting jobs.

Given all this, you can find the best high-memory partitions (the partition and node names are given with `scontrol show nodes`) and then submit to those queues. It can be even easier if your HPC team has labelled a queue as "highmem", for example, but that only happens about half the time in my experience.

Now that you know the best high mem nodes, you need to know what combination of information to request in the job submission. If you have need of a single CPU and high memory, say 300Gb, how do you request? This is an important questions because in HPC systems often a node may have say 300Gb, but this is distributed to say 6 CPUs, each of which has fast access to 50Gb of RAM (note that I'm always talking about RAM here when I say memory, not storage space). So the big question is, given this setup, can I still access the full memory of 300Gb on the node (assuming it's all available right now) if I only request a single CPU?

It seems most of the time the answer is yes. If it's a NUMA (Non-Uniform Memory Access) system, then memory is *physically connected* to a given CPU. This means memory can be local if it's physically tied to a CPU, or remote if it's tied to another CPU. Other CPUs can still access that remote memory, it's just slower, so it comes with a performance penalty. The best performacne will always come from using local memory here. NUMA systems are apparently common in HPC, so it's likely what you're facing, but I give no guarantees! If memory is a bigger bottleneck than speed, this is no problem. So most of the time, the answer is to request mem=300G, tasks=1 and cpus-per-task=1, and you can still get all the memory you need (it may also request the associated CPUs to reserve the memory associated with them, meaning they'll remain idle).

Now what if you do indeed want to use multiple CPUs? As long as your code can handle this, we can instead specify a higher cpus-per-task. You can add a specific amount per cpu if you know, e.g. mem-per-cpu=50G, but otherwise sticking to a simple mem=300G will ensure that amount is reserved and it *should* all be availabel to you, but perhaps with some performance penalty if you have to steal from other CPUs. If you do use mem-per-cpu, total mem is just mem-per-cpu * cpus-per-task. It's easy for job scripts to fail beacuse you have requested more per cpu than is available on a node. This is why it's really important to check `scontrol show nodes` and can be helpful to just stick to setting `mem` if you're not sure. 

Finally, what if you set both `mem` and `mem-per-cpu`? SLURM selects the most restrictive option. So it's worth making sure you haven't accidentally left a `mem-per-cpu` setting in your job submission script when you updated `mem`, as it will limit your memory, no matter how high `mem` goes.