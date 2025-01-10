---
layout: single
title:  "Using GPUs in an HPC setting"
date:   2025-01-09
category:
    HPC
tags:
  - GPU
  - HPC
  - SLURM
---

Whenever I've seen instructions on how to use a GPU on an HPC cluster, it almost always just tells you to use the `--gres` flag, and then not much else. Maybe it expands that a bit with more info on what you can request by adding `SBATCH --gres=<resource>:<number>` to the top of a submission script, like `#SBATCH --gres=gpu:RTX2080:1`. I still don't find this too helpful. When I first looked at this I had so many more questions. Is that really *all* I need to do? How do I know the resource and the number? What if I don't care which resource I use? Will GPU-enabled code just work if I run it on a machine with these resources?

So, let's break down a few of these.

## Requesting a node

`--gres` stands for "generic resource scheduling", and we use it to request GPUs. You might be thinking this isn't as specific as `--cpus-per-task`, for example, and you'd be right. You can actually use it to request other "generic" resources, like storage or hardware. Use of these or GPUs themselves often involves a more custom or specific request, so the word "generic" is a bit confusing here. But it's for this reason that we need to specifically say we want a GPU. 

Following that we need to give more information. I said above that often HPC docs will say `--gres=<resource>:<number>`, but it's easier to write it as `--gres=<resource>:<type>:<count>`, where type is _**optional**_ (this is important), and count is how many you want. The resource is `gpu`, and we don't know the resource off-the-bat, but it's optional, so we can omit it. That leaves us with count, which we can start at 1. So a very basic request for resource could use the flag `--gres=gpu:1`. 

That's not all we need, however. Not everything we do is going to actually be done on the GPU. We need to load data onto it, probably perform some calculations before and after this that aren't optimised for the GPU, and we need to manage the GPU itself. For all this we *still need CPUs*. Some sites recommend requesting 4-8x the number of CPUs as your request GPUs. This is because GPUs are the fast expensive resource, and we don't want our ability to feed them data via CPUs to be a bottleneck. However, recommendations vary a lot by sites and depend on the workload. You can always start small and see whether it needs to increase, while checking job resource use with `seff` if it's available on your system. It's important to get things running efficiently while not hogging resources! 

So, combining everything, we can request a short interactive GPU node with `salloc --time=00:30:00 --nodes=1 --ntasks=1 --tasks-per-node=1 --mem=20G --job-name=gputest --gres=gpu:1 --cpus-per-task=4`. We then need to actually use the node, which as usual do by something like `srun --pty /bin/bash` or sshing into the node when the name is returned upon successful allocation.

## Checking it's worked

Great. Now we have a node with GPUs and we are using a bash terminal on that node. How do we know the GPUs are there, good and visible, ready to be used? On most HPC systems you will have CUDA or NVIDIA modules ready to be loaded, and if you don't then that's a conversation to have with your HPC team. Assuming you do, we can do something simple like:

```
module load nvidia/cuda/12.6/compilers
nvidia-smi
```

This should print out a nice little table showing the GPU resources available.

## Does it just work?

If you have something which is capable of utilising GPUs, like pytorch, then you need to make sure it's actually using them at the right time by pushing the model and the data to the GPU, as below:

```
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model.to(device)
inputs = inputs.to(device)
```

Once you've done this, then it should indeed "just work". This doesn't necessarily mean it will just work *well* though. It's up to you to ensure, for example, that the batch size is big enough for a GPU to be worth it, or that there aren't a bunch of CPU-based operations interspersed with your GPU-based operations, leading to a lot of data shuffling back and forth, slowing down your code. 

## How do I get more specific?

`scontrol show nodes` will give you more information than you wanted to know about the nodes and what's available. Searching through these ("/gpu" and then "n" in vim to iterate) will show you what nodes are available with GPUs. Within these you will see a section which says "Gres". This is the one. If, for example, it says "Gres=gpu:RTX2080:4", then you have four RTX2080 GPUs available on the node. Elsewhere, for the node, it may say something like "CPUTot=40" and elsewhere "RealMemory=410234", meaning there are also 40 CPUs and around 410Gb memory on the machine. The amount of RAM dedicated specifically to the GPUs is predefined, but not visible under `scontrol show nodes` as SLURM doesn't directly manage it. Instead, you'll have to look up "RTX2080" GPUs see what they come with (I believe it's 8Gb as standard). We'll see later that `nvidia-smi` can also be used when you've already been allocated the node, so you can check manually this way too.

If you're going to specify an exact resource, it's important to check that you can choose a node with the number of GPUs you're going to ask for, and the amount of memory you need. As mentioned above, GPUs will have an amount of RAM physically attached to them for very fast access, but they can access other memory on the node (read: steal from CPUs, leaving those CPUs idle as they cannot be requested) at a large performance cost. While you need to know the GPU mem, you can't specify it, and *it isn't included in the allocation from `--mem`*. This means when you request GPUs with `--gres`, you automatically get their **fixed onboard VRAM** (Video RAM, i.e. for a graphics card/GPU), but the amount (say 8Gb) isn't part of the amount of RAM you request with `--mem`: if you request a GPU, you have it's onboard VRAM as part of this. If you also pass `--mem`, then you get general **system RAM** too, which you may need for spillover from the GPU, or for CPU operations.

So, you need to check:

- that the node has your number of GPUs
- that *additional* memory you request is within `--mem`
- that the number of CPUs is within what's available on the machine

So, when I gave the example of `--gres=gpu:RTX2080:1` as a specific request, it's been done by specifying the type (RTX2080) in between the resource (gpu) and the number of GPUs (1), where the type is found by viewing the node info. If I request something like `salloc --time=12:00:00 --nodes=1 --ntasks=1 --tasks-per-node=1 --mem=64G --job-name=gpu_max --gres=gpu:RTX2080:4 --cpus-per-task=32`, then it's because I know:

- the queue (shown by `sinfo`) isn't capped below 12 hours
- the node has 4 RTX2080 GPUs
- it also has 32 CPUs available
- it has 64Gb RAM available

But I don't need to include my GPU memory needs in that request.

## Summary

There. Slightly more complicated than "just add a `--gres=<resource>:<number>` flag to your job submission script". I don't think it's surprising that these resources often go underused at universities when there's just not enough documentation to even get started!