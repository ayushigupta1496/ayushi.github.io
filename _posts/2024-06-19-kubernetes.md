---
layout: post
title: 19-06-2024
---

# KUBERNETES


- kubernetes or k8s is an open-source orchestration and cluster management for container-based applications maintained by the Cloud Native     Computing Foundation.
- Kubernetes helps in scaling applications,load-balancing, self-healing, and rolling updates, making it well-suited for running containers.

**Kubernetes Installation script**
 {% highlight ruby %}
    sudo apt-get update -y
    sudo apt-get install -y apt-transport-https ca-certificates curl

    sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

    sudo mkdir -p -m 755 /etc/apt/keyrings

    sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

    sudo apt-get update

    sudo apt-get install -y kubelet kubeadm kubectl

    sudo apt-mark hold kubelet kubeadm kubectl
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
    sudo swapoff -a
    sudo modprobe overlay
    sudo modprobe br_netfilter

    sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    net.ipv4.ip_forward = 1
    EOF

    sudo sysctl --system

    sudo tee /etc/modules-load.d/containerd.conf <<EOF
    overlay
    br_netfilter
    EOF

    sudo sysctl --system

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    sudo apt update

    sudo apt install -y containerd.io

    mkdir -p /etc/containerd

    containerd config default | sudo tee /etc/containerd/config.toml

    sudo systemctl restart containerd

    sudo systemctl enable containerd

    sudo systemctl enable kubelet

    kubectl version

    sudo kubeadm config images pull --cri-socket /run/containerd/containerd.sock --kubernetes-version v1.30.0

    sudo kubeadm init   --pod-network-cidr=10.244.0.0/16   --upload-certs --kubernetes-version=v1.30.0  --control-plane-endpoint=ip --ignore-preflight-errors=all  --cri-socket /run/containerd/containerd.sock

    mkdir -p $HOME/.kube

    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

    sudo chown $(id -u):$(id -g) $HOME/.kube/config

    kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml
 {% endhighlight %}


 **Kubernetes Architecture**
 - All the important components of control-plane(master node) & worker node of kubernetes.
 - Following are the components of Kubernetes Master Machine.
      **control-plane(master-node)** - It is responsible for managing the cluster ,in production environment it works on multiple nodes that span across several data zones.api-server - It stores the configuration information which can be used by each of the nodes in the cluster.   
      1. api-server is the primary interface between the control-plane and the rest of the cluster ,it exposes a api that allow client to interact with control plane and submit request to manage the cluster
      2. etcd - etcd is a distributed key:value store,it store the cluster persistent state .It is used by the api-server and other components of the control plane to store and retrieve information about cluster's.
      3. Scheduler- the scheduler is responsible for scheduling pods onto the worker nodes in the cluster.It uses information about the resources required by the pods and the available resources on the worker node.
      4. Controller-manager - The controller manager is responsible for running controllers that manage the state of the cluster




  **Worker-node** - These nodes run the containerized application workload,the containerized application run in a pod.pods are the smallest deployable unit in kubernetes,pod host one or more containers 
   **The core components of kubernetes that run on the worker node are**
      1. Kubelet - the kubelet is a daemon that run on each worker node,it is responsible for communicating with control plane.It receieve information from control plane for which pod to run on the node and ensures that the desired state of the pod is maintained
      2. Container-Runtime - The container runtime runs the container on worker nodes,it is responsible for pulling the container image from the registry,starting and stopping the containers and managing the container's resources
      3. Kube-proxy - The kube-proxy is the network proxythat runs on each worker node,it is responsible for routing traffic to the correct pods and it also provide load-balancing for the pods and ensures traffic is distributed evenly across the pods



 