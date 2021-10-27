They are likely to have come from NGC. 
However, there are very few mysteries here, you can list the packages inside the containers in 
an interactive shell for instance and follow the example NSC has on https://www.nsc.liu.se/support/systems/tetralith-GPU-user-guide/. 
Add to that example the NCCL and cuDNN packages, and a Conda installation + whatever packages you need and you should have a more or less functional equivalent of the NGC containers, 
at least with respect to DL stuff. 
However, you will of course not have any of the QA NVIDIA does going into it, and if you want to run regular MPI parallel stuff on the container with GDR working between nodes, you are in for quite a bit of work.

These are images from NGC, and getting these images can be accomplished with:

```
singularity pull docker://nvcr.io/nvidia/pytorch:21.08-py3
```

It was built from the NGC docker container 21.03 and 21.07:

- See [https://ngc.nvidia.com/catalog/containers/nvidia:pytorch/tags](https://ngc.nttps://ngc.nvidia.com/catalog/containers/nvidia:pytorch/tags)

For this bootcamp, there is no definition file. Just build from NGC.

```
singularity build pytorch_21.03.sif docker://nvcr.io/nvidia/pytorch:21.03-py3
```

TODO: **The .sif file sizes are different just from time of builds**

Pointers to check this:

> also, depending on which CUDA lib version you have you might want to check against 
> https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html

> depending on the environment , you might need to double check what CUDA version is there 
> https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel_21-03.html#rel_21-03

