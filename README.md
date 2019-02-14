# MNIST training using nvidia + docker

We use the classical [MNIST](http://yann.lecun.com/exdb/mnist/) machine learning problem to bench hardware.

## Keras

[Keras](https://keras.io/) framework is used in this part. 

### CPU

To build the CPU image (behind a proxy) : 
    
    docker build -t keras-mnist-cpu \
      --build-arg http_proxy=${http_proxy} \
      --build-arg https_proxy=${https_proxy} \
      --build-arg ftp_proxy=${ftp_proxy} \
      --build-arg no_proxy=${no_proxy} \
      keras/cpu

and run it :

    docker run \
           -e http_proxy=${http_proxy} \
           -e https_proxy=${https_proxy} \
           keras-mnist-cpu

### GPU

First we need to install docker-nvidia, see [here](https://github.com/NVIDIA/nvidia-docker). On CentOS 7 :

    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | \ 
    sudo tee /etc/yum.repos.d/nvidia-docker.repo
    sudo yum install -y nvidia-docker2
    sudo pkill -SIGHUP dockerd

#### Capability <= 3.0 

We have to compile tensorflow from sources to enable old GPU capabilities :

    docker build -t keras-mnist-old-gpu \
      --build-arg http_proxy=${http_proxy} \
      --build-arg https_proxy=${https_proxy} \
      --build-arg ftp_proxy=${ftp_proxy} \
      --build-arg no_proxy=${no_proxy} \
      keras/old-gpu
	
and run it :

    docker run --runtime=nvidia \
           -e http_proxy=${http_proxy} \
           -e https_proxy=${https_proxy} \
           keras-mnist-old-gpu

#### Capability > 3.0 

To build a GPU image (behind a proxy) : 
    
    docker build -t keras-mnist-gpu \
      --build-arg http_proxy=${http_proxy} \
      --build-arg https_proxy=${https_proxy} \
      --build-arg ftp_proxy=${ftp_proxy} \
      --build-arg no_proxy=${no_proxy} \
      keras/gpu

and run it :

    docker run --runtime=nvidia \
           -e http_proxy=${http_proxy} \
           -e https_proxy=${https_proxy} \
           keras-mnist-gpu

## Caffe

[Caffe](http://caffe.berkeleyvision.org/) framework is used in this part.

### CPU

To build the CPU image (behind a proxy) :

    docker build -t caffe-mnist-cpu \
      --build-arg http_proxy=${http_proxy} \
      --build-arg https_proxy=${https_proxy} \
      --build-arg ftp_proxy=${ftp_proxy} \
      --build-arg no_proxy=${no_proxy} \
      caffe/cpu

and run it :

    docker run \
           -e http_proxy=${http_proxy} \
           -e https_proxy=${https_proxy} \
           caffe-mnist-cpu

### CUDA arch 60 cards only  

To build a GPU image (behind a proxy) :

    docker build -t caffe-mnist-gpu \
      --build-arg http_proxy=${http_proxy} \
      --build-arg https_proxy=${https_proxy} \
      --build-arg ftp_proxy=${ftp_proxy} \
      --build-arg no_proxy=${no_proxy} \
      caffe/gpu

and run it :

    docker run --runtime=nvidia \
           -e http_proxy=${http_proxy} \
           -e https_proxy=${https_proxy} \
           caffe-mnist-gpu

