# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (apt)
# pytorch       latest (pip)
# ==================================================================

FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

ARG APT_INSTALL="apt-get install -y --no-install-recommends"
ARG PIP_INSTALL="python -m pip --no-cache-dir install --upgrade"
ARG GIT_CLONE="git clone --depth 10"

RUN rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update

ARG DEBIAN_FRONTEND=noninteractive

RUN $APT_INSTALL build-essential ca-certificates cmake wget git vim

RUN $APT_INSTALL software-properties-common

RUN add-apt-repository ppa:deadsnakes/ppa

RUN apt-get update

RUN $APT_INSTALL python3.6 python3.6-dev

RUN wget -O ~/get-pip.py https://bootstrap.pypa.io/get-pip.py

RUN python3.6 ~/get-pip.py

RUN ln -s /usr/bin/python3.6 /usr/local/bin/python3
RUN ln -s /usr/bin/python3.6 /usr/local/bin/python

RUN $PIP_INSTALL setuptools
RUN $PIP_INSTALL numpy scipy nltk lmdb

RUN $PIP_INSTALL https://download.pytorch.org/whl/cu90/torch-0.4.1-cp36-cp36m-linux_x86_64.whl
RUN $PIP_INSTALL torchvision

RUN git clone https://github.com/HawkAaron/warp-transducer
WORKDIR warp-transducer
RUN mkdir build
WORKDIR build
RUN cmake ..
RUN make
ENV WARP_RNN_PATH ../build

RUN $PIP_INSTALL cffi

ENV CUDA_HOME /usr/local/cuda

WORKDIR ../pytorch_binding
RUN python setup.py install

WORKDIR ~

RUN ldconfig
RUN apt-get clean
RUN apt-get autoremove
RUN rm -rf /var/lib/apt/lists/* /tmp/* ~/*