ARG CUDA="10.1"
ARG CUDNN="8"

FROM nvidia/cuda:${CUDA}-cudnn${CUDNN}-devel-ubuntu18.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# install basics
RUN apt-get update -y \
 && apt-get install -y apt-utils git curl ca-certificates bzip2 cmake tree htop bmon iotop g++ \
 && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev libgl1-mesa-glx ffmpeg

# Install Miniconda
RUN curl -LO https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b
RUN rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/miniconda/bin:$PATH

RUN conda update -y conda

# Create a Python 3.6 environment
RUN conda install -y conda-build \
 && conda create -y --name py36 python=3.6.7 \
 && conda clean -ya

ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

RUN conda install -y ipython scipy h5py
RUN pip install requests ninja yacs cython matplotlib opencv-python tqdm overrides

# Install PyTorch 1.0 Nightly
ARG CUDA
RUN conda install pytorch==1.4.0 torchvision==0.5.0 cudatoolkit=${CUDA} -c pytorch \
 && conda clean -ya

# install pycocotools
RUN git clone https://github.com/cocodataset/cocoapi.git \
 && cd cocoapi/PythonAPI \
 && python setup.py build_ext install

# install apex
RUN git clone https://github.com/NVIDIA/apex.git \
 && cd apex \
 && python setup.py install --cuda_ext --cpp_ext

# install PyTorch Detection
RUN git clone https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch.git \
 && cd Scene-Graph-Benchmark.pytorch \
 && python setup.py build develop

WORKDIR /Scene-Graph-Benchmark.pytorch
