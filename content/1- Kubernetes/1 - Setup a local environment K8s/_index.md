---
title: "Setup a local environment for Kubernetes"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

#### Install Docker Desktop

First yo can follow this video to install docker desktop in your own machine
[Install Docker Desktop Tutorial](https://www.youtube.com/watch?v=5RQbdMn04Oc)

#### Install Minikube

Then follow this step to install minikube in your own machine depend on your operating system.

[Step to install Minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)

![Install minikube](./images/install-minikube.png)

#### Verify minikube installation

```shell
minikube start
```

```shell
minikube status
```

#### Install kubectl

You can follow this step to install kubectl in your own machine depend on your operating system.
[Step to install kubectl](https://kubernetes.io/docs/tasks/tools/)

#### Verify all installation

```shell
kubectl get pods -A
```

![Verify Installation Kubectl](images/_index.png)
