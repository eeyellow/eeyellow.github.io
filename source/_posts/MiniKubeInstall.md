---
title: MinikubeInstall
date: 2024-07-17 10:12:51
tags: [DevOps, k8s]
---
# 在Windows WSL2 上安裝地端Kubernetes (Minikube)

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
#NAMESPACE              NAME                                        READY   STATUS    RESTARTS        AGE
#kube-system            coredns-7db6d8ff4d-v89pt                    1/1     Running   2 (7m43s ago)   8d
#kube-system            etcd-minikube                               1/1     Running   2 (7m48s ago)   8d
#kube-system            kube-apiserver-minikube                     1/1     Running   2 (7m38s ago)   8d
#kube-system            kube-controller-manager-minikube            1/1     Running   2 (7m48s ago)   8d
#kube-system            kube-proxy-sdjmt                            1/1     Running   2 (7m48s ago)   8d
#kube-system            kube-scheduler-minikube                     1/1     Running   2 (7m48s ago)   8d
#kube-system            storage-provisioner                         1/1     Running   4 (7m28s ago)   8d
#kubernetes-dashboard   dashboard-metrics-scraper-b5fc48f67-gzqrj   1/1     Running   2 (7m48s ago)   8d
#kubernetes-dashboard   kubernetes-dashboard-779776cb65-8df95       1/1     Running   3 (7m27s ago)   8d
```
