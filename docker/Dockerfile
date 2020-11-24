ARG CUDA="10.1"
ARG CUDNN="8"

FROM nvidia/cuda:${CUDA}-cudnn${CUDNN}-devel-ubuntu18.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# install basics
RUN apt-get update -y \
 && apt-get install -y apt-utils git curl ca-certificates bzip2 cmake tree htop bmon iotop g++ wget \
 && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev libgl1-mesa-glx ffmpeg \
 && apt-get install -y --allow-downgrades --allow-change-held-packages libnccl2=2.7.3-1+cuda10.1 libnccl-dev=2.7.3-1+cuda10.1 \
 && apt-get clean -y

RUN apt-get install -y software-properties-common && add-apt-repository -y ppa:deadsnakes/ppa && apt-get install -y python3-pip

RUN apt-get update -y
RUN pip3 install --upgrade pip
RUN pip3 install scipy h5py ipython ninja yacs cython matplotlib tqdm opencv-python overrides

RUN pip3 install torch==1.7.0+cu101 torchvision==0.8.1+cu101 -f https://download.pytorch.org/whl/torch_stable.html

RUN git clone https://github.com/cocodataset/cocoapi.git && git clone https://github.com/NVIDIA/apex.git && git clone https://github.com/vrdn-23/Scene-Graph-Benchmark.pytorch.git sg_docker

ARG driver_version="450.51.06"

RUN apt-get update -y \
&&  apt-get -y install nvidia-settings=${driver_version}-0ubuntu1 libxnvctrl0=${driver_version}-0ubuntu1 libnvidia-decode-450=${driver_version}-0ubuntu1 libnvidia-encode-450=${driver_version}-0ubuntu1 \
    libnvidia-common-450=${driver_version}-0ubuntu1 libnvidia-compute-450=${driver_version}-0ubuntu1 libnvidia-compute-450=${driver_version}-0ubuntu1 \
    cuda-drivers-450=${driver_version}-1 libnvidia-fbc1-450=${driver_version}-0ubuntu1 libnvidia-gl-450=${driver_version}-0ubuntu1 libnvidia-ifr1-450=${driver_version}-0ubuntu1 \
    nvidia-modprobe=${driver_version}-0ubuntu1 nvidia-driver-450=${driver_version}-0ubuntu1 xserver-xorg-video-nvidia-450=${driver_version}-0ubuntu1 \
    libnvidia-cfg1-450=${driver_version}-0ubuntu1 nvidia-utils-450=${driver_version}-0ubuntu1 libnvidia-extra-450=${driver_version}-0ubuntu1 nvidia-dkms-450=${driver_version}-0ubuntu1 \
    nvidia-kernel-common-450=${driver_version}-0ubuntu1 nvidia-kernel-source-450=${driver_version}-0ubuntu1 nvidia-compute-utils-450=${driver_version}-0ubuntu1
RUN apt-get clean -y


# install pycocotools
RUN cd cocoapi/PythonAPI \
 && python3 setup.py build_ext install

ENV TORCH_CUDA_ARCH_LIST="Volta"
# install apex
RUN cd apex \
 && python3 setup.py install --cuda_ext --cpp_ext
#install PyTorch Detection

ARG FORCE_CUDA="1"
ENV FORCE_CUDA=${FORCE_CUDA}
ENV CUDA_VISIBLE_DEVICES="0"
#RUN cd sg_docker \
# && python3 setup.py build develop

WORKDIR /sg_docker
