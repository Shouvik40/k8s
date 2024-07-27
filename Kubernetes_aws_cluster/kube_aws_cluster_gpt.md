# Kubernetes Installation on AWS Linux Instances

This guide outlines the steps to install Kubernetes using `kubeadm` on AWS Linux instances. It covers setting up a Kubernetes cluster with one master node and two worker nodes.

## Table of Contents

- [Prerequisites](#prerequisites)
- [1. Update and Install Docker](#1-update-and-install-docker)
- [2. Install Kubernetes Components](#2-install-kubernetes-components)
- [3. Initialize the Kubernetes Cluster](#3-initialize-the-kubernetes-cluster)
- [4. Install a Pod Network](#4-install-a-pod-network)
- [5. Join Worker Nodes to the Cluster](#5-join-worker-nodes-to-the-cluster)
- [6. Verify the Cluster](#6-verify-the-cluster)
- [Additional Notes](#additional-notes)

## Prerequisites

- **Instances:**
  - **Master Node:** `master`
  - **Worker Nodes:** `node1` and `node2`
- **Operating System:** AWS Linux

## 1. Update and Install Docker

### On All Nodes

1. Switch to the root user:

    ```bash
    sudo su
    ```

2. Update the system and install Docker:

    ```bash
    yum update -y
    yum install -y docker
    ```

3. Start and enable Docker:

    ```bash
    systemctl start docker
    systemctl enable docker
    ```

## 2. Install Kubernetes Components

### Add Kubernetes Repository

1. Create a Kubernetes repository configuration file on all nodes:

    ```bash
    sudo tee /etc/yum.repos.d/kubernetes.repo <<EOF
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
           https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    EOF
    ```

### Install `kubeadm`, `kubelet`, and `kubectl`

1. Install the Kubernetes components:

    ```bash
    yum install -y kubelet kubeadm kubectl
    ```

2. Start and enable the `kubelet` service:

    ```bash
    systemctl enable kubelet
    systemctl start kubelet
    ```

## 3. Initialize the Kubernetes Cluster

### On the Master Node

1. Initialize the Kubernetes cluster:

    ```bash
    sudo kubeadm init --pod-network-cidr=10.244.0.0/16
    ```

### Post-Initialization Steps

1. After running the `kubeadm init` command, you will receive instructions to set up `kubectl`. Execute these commands as a regular user (not as `root`):

    ```bash
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```

## 4. Install a Pod Network

To enable communication between pods, install a network plugin. For example, to install Flannel:

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
