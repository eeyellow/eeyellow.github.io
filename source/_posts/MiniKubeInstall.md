---
title: Kubernetes 地端架設
date: 2024-07-17 10:12:51
tags: [DevOps, k8s]
---
- [在Windows WSL2 上安裝地端Kubernetes (Minikube)](#在windows-wsl2-上安裝地端kubernetes-minikube)
    - [Prerequisite](#prerequisite)
    - [Install Minikube](#install-minikube)
    - [Install Kubectl](#install-kubectl)
    - [Running Minikube](#running-minikube)
    - [Install Helm](#install-helm)
    - [Install Kompose](#install-kompose)
    - [Check Version](#check-version)

# 在Windows WSL2 上安裝地端Kubernetes (Minikube)

### Prerequisite

{% post_link DockerTutorial 安裝WSL2 + Docker %}


### Install Minikube

```bash
# Download the latest Minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

# Make it executable
chmod +x ./minikube

# Move it to your user's executable PATH
sudo mv ./minikube /usr/local/bin/

#Set the driver version to Docker
minikube config set driver docker
```

### Install Kubectl

```bash
# Download the latest Kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Make it executable
chmod +x ./kubectl

# Move it to your user's executable PATH
sudo mv ./kubectl /usr/local/bin/
```

### Running Minikube

```bash
minikube start

kubectl config use-context minikube

# Start minikube again to enable kubectl in it
minikube start

kubectl get pods -A
# NAMESPACE              NAME                                        READY   STATUS    RESTARTS        AGE
# kube-system            coredns-7db6d8ff4d-v89pt                    1/1     Running   2 (7m43s ago)   8d
# kube-system            etcd-minikube                               1/1     Running   2 (7m48s ago)   8d
# kube-system            kube-apiserver-minikube                     1/1     Running   2 (7m38s ago)   8d
# kube-system            kube-controller-manager-minikube            1/1     Running   2 (7m48s ago)   8d
# kube-system            kube-proxy-sdjmt                            1/1     Running   2 (7m48s ago)   8d
# kube-system            kube-scheduler-minikube                     1/1     Running   2 (7m48s ago)   8d
# kube-system            storage-provisioner                         1/1     Running   4 (7m28s ago)   8d
# kubernetes-dashboard   dashboard-metrics-scraper-b5fc48f67-gzqrj   1/1     Running   2 (7m48s ago)   8d
# kubernetes-dashboard   kubernetes-dashboard-779776cb65-8df95       1/1     Running   3 (7m27s ago)   8d

minikube dashboard
# 用瀏覽器開啟儀表板連結
(ctrl + z)
bg # 讓dashboard在背景執行
```

### Install Helm

```bash
curl --output helm-linux-amd64.tar.gz https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz

tar -zxvf helm-linux-amd64.tar.gz

sudo mv linux-amd64/helm /usr/local/bin/helm

rm helm-linux-amd64.tar.gz

rm -rf linux-amd64
```

### Install Kompose

```bash
curl -L https://github.com/kubernetes/kompose/releases/download/v1.34.0/kompose-linux-amd64 -o kompose

chmod +x kompose

sudo mv ./kompose /usr/local/bin/kompose
```

### Check Version

```bash

minikube version
# minikube version: v1.33.1
# commit: 5883c09216182566a63dff4c326a6fc9ed2982ff

kubectl version
# Client Version: v1.30.2
# Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

helm version
# version.BuildInfo{Version:"v3.4.2", GitCommit:"23dd3af5e19a02d4f4baa5b2f242645a1a3af629", GitTreeState:"clean", GoVersion:"go1.14.13"}

kompose version
# 1.34.0 (cbf2835db)
```