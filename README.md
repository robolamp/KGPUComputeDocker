# KGPUComputeDocker
Docker image for Deep Learning. Based on [floydhub/dl-docker](https://github.com/floydhub/dl-docker).

### What's inside 

* Ubuntu 16.04
* [CUDA 8.0](https://developer.nvidia.com/cuda-toolkit)
* [cuDNN v6](https://developer.nvidia.com/cudnn)
* [Tensorflow](https://www.tensorflow.org/)
* [Keras](http://keras.io/)
* [iPython/Jupyter Notebook](http://jupyter.org/)
* [Numpy](http://www.numpy.org/)
* [SciPy](https://www.scipy.org/) 
* [Pandas](http://pandas.pydata.org/)
* [Scikit Learn](http://scikit-learn.org/) 
* [Matplotlib](http://matplotlib.org/)
* [OpenCV](http://opencv.org/)

TODO:
* [Torch](http://torch.ch/) (includes nn, cutorch, cunn and cuDNN bindings)
* Some Python libraries for NLP

## How-to
### Prerequisites
0. Some Linux distro (compatibility checked with Gentoo and Ubuntu), NVIDIA GPU 
1. Docker https://docs.docker.com/engine/installation/
2. NVIDIA drivers https://www.nvidia.com/Download/index.aspx?lang=en-us
3. NVIDIA-Docker https://github.com/NVIDIA/nvidia-docker

### Obtaining the Docker image

You can download the Docker image from [Docker Hub](https://hub.docker.com), or build an image locally.

#### Download the Docker image from Docker Hub

You can pull an image from [Docker Hub repository](https://hub.docker.com/r/robolamp/k-gpu-compute-docker/).

```bash
docker pull robolamp/k-gpu-compute-docker
```

#### Build the Docker image locally

1. Clone this repository 

```bash
git clone https://github.com/robolamp/KGPUComputeDocker.git
cd KGPUComputeDocker
```	 

2. Check Docker's Base Device Size parameter using [this guide](https://www.projectatomic.io/blog/2016/03/daemon_option_basedevicesize/)

We need at least 16 GB of memory

3. Build the image

```bash
docker build -t robolamp/k-gpu-compute-docker:gpu -f Dockerfile.gpu .
```	 

### Running the Docker image

Don't forget to prepare a directory to share with container using [Docker volumes](https://docs.docker.com/engine/admin/volumes/volumes/)

You can run container with an access to container's bash:

```bash
nvidia-docker run -it -p 4444:8888 -p 3003:6006 -v /sharedfolder:/root/sharedfolder robolamp/k-gpu-compute-docker:gpu bash
```

Alternatively, you can run jupiter notebook in container.

1. Prepare the password for your jupiter notebook

Run python in your terminal:
```bash
python
```
Encrypt your password using notebook.auth.security.passwd():

```python
>>> from notebook.auth import passwd
>>> passwd()
Enter password: 
Verify password: 
```
2. Launch container with notebook

Copy your password hash and insert in into the following command and run it:

```bash
nvidia-docker run -d -p 4444:8888 -p 3003:6006 -v /sharedfolder:/root/sharedfolder robolamp/KGPUComputeDocker:gpu jupyter notebook --NotebookApp.password='your_password_hash' --allow-root
```
