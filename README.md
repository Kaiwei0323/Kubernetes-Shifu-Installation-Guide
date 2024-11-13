# Kubernetes-Shifu-Installation-Guide

## Prerequisites

### Hardware Platform:
- **QCS6490**
  - CPU: Octa-Core Kryo 670 
  - GPU: Qualcomm Adreno 643

### Operating System:
- **Ubuntu 20.04**

### Dependencies:
- Docker
- K3S (Lightweight Kubernetes)

## Installation Steps
1. Switch to Admin Mode
Open a terminal and switch to admin mode:
```
su
Password: oelinux123
```

2. Install Docker
```
apt update
apt install docker.io
systemctl start docker
systemctl enable docker
usermod -aG docker $USER
```
Verify Docker Installation
```
docker --version
docker ps
```
![Screenshot from 2024-11-13 19-51-37](https://github.com/user-attachments/assets/c7c25e59-0970-4070-a471-7a67f1b509c9)

3. Install K3S
```
curl -sfL https://get.k3s.io | sh -s - --flannel-backend=host-gw
systemctl status k3s
```
![Screenshot from 2024-11-13 19-52-02](https://github.com/user-attachments/assets/477377c5-a81b-4a3c-beec-a31665368a93)

4. Install Shifu
```
kubectl apply -f https://raw.githubusercontent.com/Edgenesis/shifu/v0.57.0/pkg/k8s/crd/install/shifu_install.yml
```
Check Shifu pods are running:
```
kubectl get pod -A
```
![Screenshot from 2024-11-13 19-52-34](https://github.com/user-attachments/assets/67d6078b-6816-403d-ae2c-ac456ba4298e)

## Setting Up Sample Video Stream
1. Log into Shifu Cloud
Go to [Shifu Cloud](https://cloud.shifu.dev/#/user/login) and log in to your account.

2. Add Same Camera
Configure a sample camera with the following setting:
* Protocol: TCP
* Device Name: tcp-camera
* Device Manufacturer: generic
* Device Type: camera
* Device Model: generic
* Device Address: bj-hikcamera-01.saifai.cn:40554

3. Get Kubernetes Services
Run following command to list all services in Shifu namespace:
```
kubectl get svc -n deviceshifu
```
![Screenshot from 2024-11-13 19-52-51](https://github.com/user-attachments/assets/847c479d-8202-4ac2-bdaf-3cb7a295e6f2)

4. Expose Camera Stream
Port-forward the camera service to expose it on port 3000:
```
kubectl port-forward -n deviceshifu svc/deviceshifu-tcp-camera-service 3000:80 --address="0.0.0.0"
```

## Access camera stream with VLC
1. Install VLC
```
apt install vlc
```
2. Access Camera Stream
```
rtsp://guest:HelloShifu@localhost:3000
```

## Reference
* [Shifu GitHub Repo](https://github.com/Edgenesis/shifu)
* [Shifu Installation Guide](https://shifu.dev/docs/guides/install/install-shifu-prod)
* [TCP Protocol Setup Guide](https://docs.cloud.shifu.dev/docs/how-to/device/protocols/tcp-protocol/)
* [Shifu Cloud](https://cloud.shifu.dev/#/user/login)
