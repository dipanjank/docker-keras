# Docker-Keras

This is a shell project that builds an NVIDIA GPU-capable Python environment with Anaconda and Keras 
(with Tensorflow GPU) using Ubuntu Linux 18.04. One can then launch a Jupyter Notebook Server
or a standard Python project using the dockerized environment. 

# Host Machine Setup

The host machine needs to have 

* NVIDIA GPU Driver 
* docker-nvidia2

Make sure to upgrade docker and docker-compose to a fairly recent version.

## Install NVIDIA GPU Driver

Follow the instructions described in http://www.linuxandubuntu.com/home/how-to-install-latest-nvidia-drivers-in-linux.

## Install docker-nvidia2

Again, follow the instructions from: https://github.com/NVIDIA/nvidia-docker

Check that the NVIDIA GPU drivers are working by invoking the ``nvidia-smi`` utility from the host machine.

```bash
(base) deep@deep-TUF-GAMING-FX504GM-FX80GM:~/PycharmProjects/docker-keras$ nvidia-smi
Mon Apr 22 21:54:29 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.56       Driver Version: 418.56       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1060    Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   57C    P0    25W /  N/A |    243MiB /  3019MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      2195      G   /usr/lib/xorg/Xorg                           137MiB |
|    0      2378      G   /usr/bin/gnome-shell                          85MiB |
|    0      3536      G   ...charm-community-2019.1.1/jre64/bin/java    17MiB |
+-----------------------------------------------------------------------------+
(base) deep@deep-TUF-GAMING-FX504GM-FX80GM:~/PycharmProjects/docker-keras$ 

```

As a bonus, you now know which GPU laptop I'm using ;)

Next, pull an existing NVIDIA cuda containers and execute the same command. This verifies that docker-nvidia is working 
correctly.


```bash
 docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

You should see an output similar to above. 

# Install Jupyter, Keras and TensorFlow in Container

Build the ``keras-jupyter`` image using the ``docker-compose.yml`` in this project. It extends the official NVIDIA Cuda 10.1 
runtime image (with CUDA and CUDNN pre-installed) by installing Anaconda3 and then adds the GPU builds of
TensorFlow and Keras. It also maps the ``~/notebooks``  directory on the host machine to ``/home/ubuntu/notebooks`` in the
image so that you can save your notebooks on the host.

First launch a container based off of this image:

```bash
$ docker-compose run -p 8888:8888 keras-jupyter
```
 
 Once inside the container, bring up a notebook session.
 
 ```bash
$ jupyter-notebook --ip 0.0.0.0 --port 8888 --allow-root --no-browser &
```

And you're ready to kick off your GPU based Machine Learning Project! Navigate to http://127.0.0.1:8888 to access the Jupyter
server. The ``gpu_check.ipynb`` notebook included in this project is an example of checking TF device placement to ensure
that TensorFlow is really using the GPU.







