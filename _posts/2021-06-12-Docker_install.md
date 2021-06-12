## Docker 개발환경 구축

> 참고 사이트
>
> https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker



#### 저장소와 GPG 키 설정

```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```



#### 저장소 추가

```
curl -s -L https://nvidia.github.io/nvidia-container-runtime/experimental/$distribution/nvidia-container-runtime.list | sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
```



#### Install the `nvidia-docker2` package

```
$ sudo apt-get install -y nvidia-docker2
$ sudo systemctl restart docker
$ sudo docker run --rm --gpus all nvidia/cuda:9.0-base nvidia-smi
```

