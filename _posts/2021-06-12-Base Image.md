---
layout: post
title:  "GAN 환경 구축(Base Image)"
categories: GAN
---


# 환경 구축



## 버전

#### Cuda version			  : 10

#### tensorflow_gpu		 : 1.13.1

#### keras							: 2.2.4



## Nvidia - Docker 설치

### 도커의 이전 버전 제거

```shell
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1
docker ps -q -a -f volume={} | xargs -r
docker rm -f
sudo apt-get purge -y nvidia-docker
```



### 패키지 저장소 추가

```shell
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```



### 저장소 추가

```shell
curl -s -L https://nvidia.github.io/nvidia-container-runtime/experimental/$distribution/nvidia-container-runtime.list | sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
```



### `nvidia-docker2` package 설치

```shell
$ sudo apt-get install -y nvidia-docker2
$ sudo systemctl restart docker
$ sudo docker run --rm --gpus all nvidia/cuda:10.0-base nvidia-smi
```



## Docker File을 이용한 환경 설정

```dockerfile
FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
ARG KERAS=2.2.4
ARG TENSORFLOW=1.13.1

ENV DEBIAN_FRONTEND noninteractive

# Update the repositories within the container
RUN apt-get update

# Install Python 2 and 3 + our basic dev tools
RUN apt-get install -y \
          python-dev \
          python3-dev \
          curl \
          git \
          vim \
          python-pip \
          python-opencv \
          python3-pip \
          python-tk \ 
          python3-tk \
          wget \
          unzip

# Install Tensorflow and Keras for Python 2
RUN pip --no-cache-dir install \
         tensorflow_gpu==${TENSORFLOW} \ 
         keras==${KERAS} \
         numpy \
         scipy \
         lmdb \ 
         matplotlib==2.2.3 \ 
         pandas \
         pillow
         

# Install Tensorflow and Keras for Python 3
RUN pip3 --no-cache-dir install \
         tensorflow_gpu==${TENSORFLOW} \ 
         keras==${KERAS} \
         numpy \
         scipy \
         lmdb \
         matplotlib \
         pandas \
         pillow 
```

