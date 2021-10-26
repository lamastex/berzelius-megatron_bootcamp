# berzelius-megatron_bootcamp
This is a public repository of the megatron bootcamp for GPUs on berzelius supercomputer for AI in Sweden

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
- Then pull the sigularity containers as needed.

