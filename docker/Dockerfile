ARG CUDA="10.1"
ARG CUDNN="8"

FROM nvidia/cuda:${CUDA}-cudnn${CUDNN}-devel-ubuntu18.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# install basics
RUN apt-get update -y \
 && apt-get install -y apt-utils git curl ca-certificates bzip2 cmake tree htop bmon iotop g++ \
 && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev libgl1-mesa-glx ffmpeg

RUN apt-get install -y software-properties-common && add-apt-repository -y ppa:deadsnakes/ppa && apt-get install -y python3-pip

RUN apt-get update -y
RUN pip3 install --upgrade pip
RUN pip3 install scipy h5py ipython ninja yacs cython matplotlib tqdm opencv-python overrides

RUN pip3 install torch==1.4.0 torchvision==0.5.0

RUN git clone https://github.com/cocodataset/cocoapi.git && git clone https://github.com/NVIDIA/apex.git && git clone https://github.com/vrdn-23/Scene-Graph-Benchmark.pytorch.git sg_docker

# install pycocotools
RUN cd cocoapi/PythonAPI \
 && python3 setup.py build_ext install

# install apex
RUN cd apex \
 && python3 setup.py install --cuda_ext --cpp_ext

#install PyTorch Detection
#ARG FORCE_CUDA="1"
#ENV FORCE_CUDA=${FORCE_CUDA}
#ENV TORCH_CUDA_ARCH_LIST="Kepler;Kepler+Tesla;Maxwell;Maxwell+Tegra;Pascal;Volta;Turing"
RUN cd sg_docker \
 && python3 setup.py build develop

WORKDIR /sg_docker
