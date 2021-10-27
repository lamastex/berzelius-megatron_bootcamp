# berzelius-megatron_bootcamp
This is a public repository of the megatron bootcamp *Digital Event* for GPUs on [berzelius](https://www.nsc.liu.se/systems/berzelius/), the most powerful supercomputer for AI in Sweden today (2021-10-27).

This repository is mainly an archive for my own personal self-study and deeper dives by filling some gaps in my own knowledge. 
Anyone is welcome to use this to speed-up their own self-study. 

- See [https://gpuhackathons.org/event/enccs-nsc-megatron-bootcamp](https://gpuhackathons.org/event/enccs-nsc-megatron-bootcamp) for details of the event.
- Please Note: 
  - *This is released by NVIDIA Corporation under the Creative Commons Attribution 4.0 International (CC BY 4.0)* 
  - *This is released by OpenACC-Standard.org, in collaboration with NVIDIA Corporation, under the Creative Commons Attribution 4.0 International (CC BY 4.0).*

# 0. Setup

## 0.1. Access to Berzelius

- get access to berzelius by following their instructions in the invitation
  - read [https://www.nsc.liu.se/support/systems/berzelius-getting-started/](https://www.nsc.liu.se/support/systems/berzelius-getting-started/)
  - assuming you have access via `$ ssh -X x_raasa@berzelius1.nsc.liu.se`

## 0.2 Use thinlinc to connect to berzelius

- get thinlinc client for accessing the GUIs for jupyter via firefox, profiler, etc.
    - See [Chapter 8, Client Platforms](https://www.cendio.com/resources/docs/tag/clientplatforms.html) for instructions on how to start the client on different platforms. 
    - download from: [https://www.cendio.com/thinlinc/download](https://www.cendio.com/thinlinc/download) and run thinlinc client
      - for Linux see [https://www.cendio.com/resources/docs/tag/linuxclients.html](https://www.cendio.com/resources/docs/tag/linuxclients.html)  
      - `wget https://www.cendio.com/downloads/clients/thinlinc-client_4.13.0-2172_amd64.deb`
      - install: `$ sudo dpkg -i thinlinc-client_4.13.0-2172_amd64.deb`  
      - run: `$ /opt/thinlinc/bin/tlclient`, fill in username: `x_raasa` and server: `berzelius1.nsc.liu.se` andpress F-8 key to exit full-screen. 
      - If all goes well, you should see the thnilinc client with shell, browser and file manager as in [this image](images/ThinLincClientToberzelius.png)
    - for details read [https://www.cendio.com/resources/docs/tag/client.html](https://www.cendio.com/resources/docs/tag/client.html)

## 0.3. Clone companion repos in berzelius

You may use the `terminal` in thinlinc session or use directly `$ ssh -X x_raasa@berzelius1.nsc.liu.se` for the following steps.

Here we are using the forks by lamastex of the following original repos to make PRs upstream as needed:

- https://github.com/gpuhackathons-org/gpubootcamp.git
- https://github.com/NVIDIA/Megatron-LM.git

Since we need to make sure the above two companion repos are fit for operations in berzelius we will use the 
following strategy of creating [simplest git subrepos](https://github.com/jmnavarrol/simplest-git-subrepos)
with the following directory structure:

[berzelius-megatron_bootcamp]|README.md
                             | ...
                             |
                             |[gpubootcamp]|README.md
                             |             | ...
                             |
                             |[Megatron-LM]|README.md
                             |             | ...
                                   



```
$ ssh -X x_raasa@berzelius1.nsc.liu.se
$ cd /proj/megatron_bootcamp/users/x_raasa/
$ git clone git@github.com:lamastex/berzelius-megatron_bootcamp.git
$ cd berzelius-megatron_bootcamp
$ git clone git@github.com:lamastex/gpubootcamp.git
$ git clone git@github.com:lamastex/Megatron-LM.git
```
## 0.4. Building images needed

Singularity containers need to be built for deploymnet in berzelius. 

- [Quick Start](https://sylabs.io/guides/3.0/user-guide/quick_start.html) on a machine where you have root access. 
  - See [linux commands](commands/singularityOnLinux.md) for installing singularity on a Linux machine with root access.
- See [Overview of Singularity Interface](https://sylabs.io/guides/3.0/user-guide/quick_start.html#overview-of-the-singularity-interface) next.
- TODO: These `.sif` images need to built from their `.def` definitions as [explained here](https://chowdera.com/2021/06/20210620163226072P.html) and [briefed here](https://sylabs.io/guides/3.0/user-guide/quick_start.html#singularity-definition-files).
  - See [building singularity container for the bootcamp](build/README.md)
  - TODO: confirm the above build works in berzelius
- Then pull the sigularity containers as needed.

```
$ pwd
/proj/megatron_bootcamp/users/x_raasa/berzelius-megatron_bootcamp

$ singularity pull --arch amd64 library://lamastex/default/pytorch_21.03.sif:berzelius-20211027
$ singularity pull library://lamastex/default/pytorch_21.07.sif:berzelius-20211027
```

# 1. Agenda for Day 1

# 1. Agenda for  Day 1: October 25, 2021 (9:00 AM to 12:00 PM CEST)

*   09:00 AM: Welcome
*   09:00 AM - 09:15 AM: Connecting to a cluster
*   09:15 AM - 09:30 AM: Bootcamp Overview
*   09:30 AM - 09:50 AM: Quick start – docker | singularity | slurm
*   09:50 AM - 10:15 AM: Multi-nodes Megatron training
*   10:15 AM - 10:30 AM: Break
*   10:30 AM - 10:45 AM: Team-Up time
*   10:45 AM - 11:30 AM: Challenge overview
*   11:00 AM - 12:00 PM: Discussion & What’s up next day

## 1.1 Cluster, Overview, Quick start – docker | singularity | slurm

- Read [https://www.nsc.liu.se/support/systems/berzelius-getting-started/](https://www.nsc.liu.se/support/systems/berzelius-getting-started/) carefully and go through the Quick start guide in it.
- The above activity should take at least 2 hours if you have already gone through setting up above.

Create some directories we will need first:

```
$ pwd
/proj/megatron_bootcamp/users/x_raasa/berzelius-megatron_bootcamp

$ mkdir ./output/sv_gpt3_ckpt
$ mkdir ./profiles
```

Now get cluster resources in an interactive BASH shell, removing or adapting `--reservation=megatron-day2` as needed:

```
srun --reservation=megatron-day2 --gres=gpu:2 --pty bash -i
```

Specifically, you would ssh into berzelius1 and then srun to get resources in another node, say 12 in my case below:

```
$ ssh -X x_raasa@berzelius1.nsc.liu.se
$ cd /proj/megatron_bootcamp/users/x_raasa/berzelius-megatron_bootcamp
[x_raasa@berzelius001 berzelius-megatron_bootcamp]$ srun  --gres=gpu:2 --pty bash -i
[x_raasa@node012 berzelius-megatron_bootcamp]$
```

Note that this resource is time-limted, so you may have to `srun` again from berzelius1.


To run jupyter notebook you first create and scp a `.cert` file and follow [the first few steps here](guides/AccessingJupyterNotebooks.pdf).

Then you mostly need just these commands [as shown here](images/jupyterLabLaunchViaThinlincTerminalInberzeliusSrunGresInteractiveBASHIntoSingularity.png):

```
srun --gres=gpu:2 --pty bash -i
export SINGULARITY_BINDPATH="/proj/megatron_bootcamp/users/$(id -un)"
# singularity shell --nv pytorch_21.03.sif # use this if you are using .sif from extracted assets.tar
singularity shell --nv pytorch_21.03.sif_berzelius-20211027.sif
jupyter-lab --certfile=~/mycert.pem --ip=$(hostname) --port=9000 # use your allocated port
```

You will then see the following:

```
Singularity> jupyter-lab --certfile=~/mycert.pem --ip=$(hostname) --port=9000
[I 03:15:09.654 LabApp] jupyter_tensorboard extension loaded.
[I 03:15:09.667 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.8/site-packages/jupyterlab
[I 03:15:09.667 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 03:15:09.669 LabApp] [Jupytext Server Extension] NotebookApp.contents_manager_class is (a subclass of) jupytext.TextFileContentsManager already - OK
[I 03:15:09.669 LabApp] Serving notebooks from local directory: /proj/megatron_bootcamp/users/x_raasa/berzelius-megatron_bootcamp
[I 03:15:09.669 LabApp] Jupyter Notebook 6.2.0 is running at:
[I 03:15:09.669 LabApp] http://hostname:8888/?token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
[I 03:15:09.669 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 03:15:09.675 LabApp] 
    
    To access the notebook, open this file in a browser:
        file:///home/x_raasa/.local/share/jupyter/runtime/nbserver-1904518-open.html
    Or copy and paste this URL:
        http://hostname:8888/?token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

We can just open the browser in the ThinLinc client (Advanced -> Accept Risk/Continue) and paste the following URL:

```
file:///home/x_raasa/.local/share/jupyter/runtime/nbserver-1904518-open.html
```

Your thinLinc Client should have the terminal and jupyter lab in firefox running like [this](images/jupyterLabRunning.png).

Now you are ready to work on the Labs!

## 1.2 Multi-nodes Megatron training

See [slides](slides/) for the lectures.

## 1.3 Labs for Day 1

Go through the following labs in order:

- `gpubootcamp/ai/Megatron/English/Python/Start_Here.ipynb` ?? 
- I guess watch the video and figure out which notebooks were meant to be done in what order?

# 2. Agenda for Day 2: October 26, 2021 (9:00 AM to 1:30 PM CEST)

*   09:00 AM - 09:15 AM: Environment prep
*   09:15 AM - 09:45 AM: Introduction to Megatron
*   09:45 AM - 10:15 AM: Tutorial part 1 – Pre-requisite
*   10:15 AM - 10:45 AM: Tutorial part 2 – Megatron’s core MPU
*   10:45 AM - 11:00 AM: Break
*   11:00 AM - 11:40 AM: Tutorial part 3 – data preprocessing
*   11:40 AM - 12:00 PM: Intro to profiling
*   12:00 PM - 12:10 PM: Ask the Experts
*   12:10 PM - 12:40 PM: Tutorial part 4 – GPT config vs GPUs performance
*   12:40 PM - 01:10 PM: Challenge overview
*   01:10 PM - 01:30 PM: Discussion & What’s up next day

## 2.1 Labs for Day 2

See [slides](slides/) for the lectures.

Go through the following labs in order:

- `gpubootcamp/ai/Megatron/English/Python/Start_Here.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Megatron-LM/tools/openwebtext/Lab1-1_Website_scraping.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Lab1-2_EstimateComputeDaysNeeded.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Lab1-3_MegatronFundementals.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Lab1-4_GPT_vocab_merge_files.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Lab1-5_jsonfy_and_process2mmap.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Lab1-6_Observe_GPT_runs_vs_performance.ipynb`

# 3. Agenda for Day 3: October 27, 2021 (9:00 AM to 2:00 PM CEST)

*   09:00 AM - 09:15 AM: Recap and Overview of Day 3
*   09:15 AM - 09:30 AM: About acquiring your own
*   09:30 AM - 10:10 AM: Tutorial part 1 – data cleaning & filter
*   10:10 AM - 10:30 AM: Mini challenge – approaching ground-truth
*   10:30 AM - 11:00 AM: Tutorial part 2 – train your own GPT tokenizer
*   11:00 AM - 11:30 AM: Tutorial part 3 – data preprocessing
*   11:30 AM - 12:00 PM: Mini challenge – customize preprocessing script
*   12:00 PM - 12:30 PM: Break
*   12:30 PM - 12:50 PM: Tutorial part 4 – recap on Megatron model parallelism
*   12:50 PM - 01:20 PM: Challenge – Go BIG or go home
*   01:20 PM - 01:40 PM: Ask the Experts with NVIDIA data scientists on what's next for NLP
*   01:40 PM - 02:00 PM: Discussion & Final remarks

## 3.1 Labs for Day 3

See [slides](slides/) for the lectures.

Go through the following labs in order:

- `gpubootcamp/ai/Megatron/English/Python/Start_Here.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Megatron-LM/tools/openwebtext/Lab1-1_Website_scraping.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Megatron-LM/tools/openwebtext/Lab2-1_acquiring_data.ipynb`
- `gpubootcamp/ai/Megatron/English/Python/jupyter_notebook/Megatron-LM/tools/openwebtext/Lab2-2_SentenceBoundary_and_Deduplicate.ipynb`
- `...`

# CC

- See [https://gpuhackathons.org/event/enccs-nsc-megatron-bootcamp](https://gpuhackathons.org/event/enccs-nsc-megatron-bootcamp) for details of the event.
- Please Note: 
  - *This is released by NVIDIA Corporation under the Creative Commons Attribution 4.0 International (CC BY 4.0)* 
  - *This is released by OpenACC-Standard.org, in collaboration with NVIDIA Corporation, under the Creative Commons Attribution 4.0 International (CC BY 4.0).*
