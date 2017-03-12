# Dockerfile for the py-faster-rcnn framework (based on caffe)
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
        python-opencv \
        python-numpy \
        python-pip \
        python-skimage \
        python-scipy \
        python-tk \
        python-zmq \
        software-properties-common && \
    add-apt-repository -y ppa:ubuntu-toolchain-r/test && \
    apt-get update && \
    apt-get -y install gcc-5 g++-5 && \
    rm -rf /var/lib/apt/lists/*

ENV CAFFE_ROOT=/opt/py-faster-rcnn
WORKDIR $CAFFE_ROOT

# IP is hardcoded, DNS would not work with the local DNS Server.
RUN git clone https://github.com/rbgirshick/py-faster-rcnn.git . && \
    for req in $(cat caffe-fast-rcnn/python/requirements.txt) pydot; do pip install $req; done && \
    pip install easydict  && \
    cd lib && make && cd ../caffe-fast-rcnn && \
    make -j"$(nproc)" && make pycaffe

# Define externally mounted volume
VOLUME ["/data/projects/"]

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR $CAFFE_ROOT
