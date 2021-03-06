# mayhem-pegasus

Useful docker images for deep learning and machine learning in general.

Current version: Pytorch, tensorflow-gpu, tensorboardx and usual useful python libraries (numpy, pandas, dask, sklearn, matplotlib, seaborn)

To build images (don't forget the dot at the ends):

```
docker build -f ./Dockerfile -t mayhem-pegasus:<TAG> .
```

To run a jupyter lab session and mounting a shared folder (host: `./mnt` will be paired with `/io` in the container) :

```
docker run -u $(id -u):$(id -g)  -v $PWD/mnt:/io -p 0.0.0.0:<local jupyter port>:8888 -p 0.0.0.0:<local tensorboard port>:6888 -it --runtime=nvidia --rm mayhem-pegasus:<TAG>
```
that is
```
docker run -u $(id -u):$(id -g)  -v $PWD/mnt:/io -p 0.0.0.0:8888:8888 -p 0.0.0.0:6888:6888 -it --runtime=nvidia --rm mayhem-pegasus:<TAG>
```


Or, with new `nvidia-docker` command,
```
docker run -u $(id -u):$(id -g)  -v $PWD/mnt:/io -p 0.0.0.0:<local jupyter port>:8888 -p 0.0.0.0:<local tensorboard port>:6006 -it --gpus <device> --rm mayhem-pegasus:<TAG>
```

where `<device>` is the device number. If you want to use multiple devices, see usage [examples](https://github.com/NVIDIA/nvidia-docker#usage).

#### examples:
`--gpus 1` to use only first one GPU

`--gpus '"device=1,2"'` to use devices 1 and 2

`--gpus all` to use all devices

that is
```
docker run -u $(id -u):$(id -g)  -v $PWD/mnt:/io -p 0.0.0.0:8888:8888 -p 0.0.0.0:6888:6006 -it --gpus '"device=1,2"'` --rm mayhem-pegasus:<TAG>
```



## Prerequisites

The provider docker file / container suppose NVidia GPU usage. You need to install nvidia-docker2 to be able to use `--runtime=nvidia`, see the following [installation instructions](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)).

## Known issues

Autocompletion in Jupyter might be veeeery slow or inactive. This might be cause by issues with `jedi` (see many issues in jupyter repo on this matter). To deactivate `jedi` and get autocompletion in the notebooks, you can run the following magic:

```%config Completer.use_jedi = False```


## Other useful docker commands

*  to get a bash shell in the container

```
docker exec -it <container name> /bin/bash
```