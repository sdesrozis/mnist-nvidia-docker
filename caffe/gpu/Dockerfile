FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Sylvain Desroziers <sylvain.desroziers@gmail.com>"

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-setuptools \
        python-scipy && \
    rm -rf /var/lib/apt/lists/*

ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT

RUN git clone -b 1.0 --depth 1 https://github.com/BVLC/caffe.git . && \
    pip install --upgrade pip 
RUN cd python && for req in $(cat requirements.txt) pydot; do pip install $req; done
RUN git clone https://github.com/NVIDIA/nccl.git && \
    cd nccl && \
    make -j install && \
    cd .. && rm -rf nccl
RUN mkdir build && cd build && \
    cmake -DCUDA_ARCH_NAME=Manual -DCUDA_ARCH_BIN=”60” -DUSE_CUDNN=1 -DUSE_NCCL=1 .. && \
    make -j 4

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

RUN data/mnist/get_mnist.sh
RUN examples/mnist/create_mnist.sh

CMD examples/mnist/train_lenet.sh
