# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.7    (apt)
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

RUN $APT_INSTALL python3.7 python3.7-dev

RUN wget -O ~/get-pip.py https://bootstrap.pypa.io/get-pip.py

RUN python3.7 ~/get-pip.py

RUN ln -s /usr/bin/python3.7 /usr/local/bin/python3
RUN ln -s /usr/bin/python3.7 /usr/local/bin/python

RUN $PIP_INSTALL setuptools
RUN $PIP_INSTALL numpy scipy nltk lmdb

RUN $PIP_INSTALL torch torchvision

RUN $GIT_CLONE https://github.com/NVIDIA/apex.git
WORKDIR apex
RUN python setup.py install --cuda_ext --cpp_ext
WORKDIR ~

RUN ldconfig
RUN apt-get clean
RUN apt-get autoremove
RUN rm -rf /var/lib/apt/lists/* /tmp/* ~/*
