# Dockerfile for the ssd framework (based on caffe)
# Created on 2016-12-07 by Matthias Kardel (mkardel@gmail.com)

FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu14.04
LABEL maintainer "Matthias Kardel <mkardel@gmail.com>"

# Install packages without having to deal with pop-ups
ARG DEBIAN_FRONTEND=noninteractive

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
        libopenblas-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-skimage \
        python-scipy \
        software-properties-common && \
    add-apt-repository -y ppa:ubuntu-toolchain-r/test && \
    apt-get update && \
    apt-get -y install gcc-5 g++-5 && \
    rm -rf /var/lib/apt/lists/*

ENV CAFFE_ROOT=/opt/ssd
WORKDIR $CAFFE_ROOT

RUN git clone https://github.com/mkardel/ssd.git . && \
    git checkout ssd && \
    for req in $(cat python/requirements.txt) pydot; do pip install $req; done && \
    make -j"$(nproc)" && make py && \
    mkdir -p models/VGGNet && cd models/VGGNet && \
    wget http://cs.unc.edu/~wliu/projects/ParseNet/VGG_ILSVRC_16_layers_fc_reduced.caffemodel && \
    # Root data dir (normally used for traininig data)
    mkdir -p ~/data && cd ~/data

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR /workspace
