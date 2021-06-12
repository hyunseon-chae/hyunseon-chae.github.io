# CUDA 9.0 설치



#### 다운로드

> 개발 버전이 cuda 9.0에 맞춰져 있어서 추가로 설치해주었다. 
>
> CUDA는 기존 버전을 삭제하지 않아도 동시에 여러 버전이 가능하다.

```
cd 
wget https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda_9.0.176_384.81_linux-run

# we will extract it to $HOME directory
cd
chmod +x cuda_9.0.176_384.81_linux-run
./cuda_9.0.176_384.81_linux-run --extract=$HOME 
sudo sh cuda-linux.9.0.176-22781540.run
```

