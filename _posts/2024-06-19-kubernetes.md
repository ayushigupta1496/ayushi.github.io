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

      1. api-server is the primary interface between the control-plane and the rest of the cluster ,it exposes a api that allow client to interact with control plane and submit request to manage the cluster.

      2. etcd - etcd is a distributed key:value store,it store the cluster persistent state .It is used by the api-server and other components of the control plane to store and retrieve information about cluster's.

      3. Scheduler- the scheduler is responsible for scheduling pods onto the worker nodes in the cluster.It uses information about the resources required by the pods and the available resources on the worker node.

      4. Controller-manager - The controller manager is responsible for running controllers that manage the state of the cluster.




  **Worker-node** - These nodes run the containerized application workload,the containerized application run in a pod.pods are the smallest deployable unit in kubernetes,pod host one or more containers.

   **The core components of kubernetes that run on the worker node are**

   1. Kubelet - the kubelet is a daemon that run on each worker node,it is responsible for communicating with control plane.It receieve information from control plane for which pod to run on the node and ensures that the desired state of the pod is maintained.

   2. Container-Runtime - The container runtime runs the container on worker nodes,it is responsible for pulling the container image from the registry,starting and stopping the containers and managing the container's resources.

   3. Kube-proxy - The kube-proxy is the network proxythat runs on each worker node,it is responsible for routing traffic to the correct pods and it also provide load-balancing for the pods and ensures traffic is distributed evenly across the pods.

 **1. KIND - pod** 
 - pods-Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
 - A Pod is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.
 - A Pod's contents are always co-located and co-scheduled, and run in a shared context.

   - Field of the yaml file
    -  apiVersion - Which version of the Kubernetes API we are using to create this object
    -  kind - What kind of object you want to create
    -  metadata - Data that helps uniquely identify the object
    -  spec - What state you desire for the object

**Commands to launch pod through ad-hoc commands**

    {% highlight ruby %}
    #command to create new pod
    kubectl run <podname> --image=image name
    kubectl get pods
    kubectl describe pod <podname>
    kubectl exec -it <container name/id> bash
    {% endhighlight %}



**yaml file to launch pod**

    {% highlight ruby %}
      apiVersion: v1
      kind: Pod
        metadata:
           name: webapp
      spec:
        containers:
          - name: nginx
            image: nginx:latest
            ports:
              - containerPort: 80
    {% endhighlight %}

**command to run manifest file for pod**

 kubectl create -f filename

    
**2. KIND - DEPLOYMENT**
 - Deployment - A Kubernetes Deployment tells Kubernetes how to create or modify instances of the pods that hold a containerized application.
 - Deployments can help to efficiently scale up and scale down the number of replica pods.
 - We can describe a desired state in a Deployment,and the deployment controller changes the actual state to the desired state.
 - We can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.
  
 **Ad-hoc commands for deployment**
    {% highlight ruby%}
    # to create new deployment
      kubectl create deployment <deploymentname> --image=image name
    # to check status of deployment
      kubectl get deployment
    # to check all the info of deployment
      kubectl describe deployment <deployment name>
    # to delete deployment
      kubectl delete deployment <deployment name>  
    # to scale up and down the replicas
      kubectl scale deployment <deployment name> --replicas=no
    # to check status of replicaset 
      kubectl get replicaset
    # to check info of replicaset 
      kubectl describe replicaset <name>            
    {% endhighlight %}

**yaml file for deployment**
{% highlight ruby%}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app
  labels:
    app: nginx
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
{% endhighlight %}

**commands to create deployment through yaml file**

 kubectl create -f filename

**command to create & run the deployment**

 kubectl apply -f filename


**Kubernetes Replication-Controller & Kubernetes-Replica-set**

1. Replication controller - A ReplicationController ensures that a specified number of pod replicas are running at any one time or it ensures that a pod or a homogeneous set of pods is always up and available.
- The pods maintained by a ReplicationController are automatically replaced if they fail, are deleted, or are terminated. For example, your pods are re-created on a node after disruptive maintenance such as a kernel upgrade. 
- Replication-controller support rolling update,it means it allows a  update to take place with zero downtime. It does this by incrementally replacing the current Pods with new ones. The new Pods are scheduled on Nodes with available resources, and Kubernetes waits for those new Pods to start before removing the old Pods.

**Manifest file for kind replication controller**

  {% highlight ruby%}
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: nginx
    spec:
      replicas: 5
      selector:
        app: nginx
      template:
        metadata:
          name: nginx
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
            ports:
            - containerPort: 80
  {% endhighlight %}


2. Replica-set - A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods, it does not support rolling update feature It is responsible for monitoring the health of the modules it manages and ensuring that the required number of replicas are always running, thus providing self-healing capabilities. ReplicaSet automatically detect and recover from module failures by creating new replicas to replace the failed ones, ensuring that the desired state of the application is maintained. With their self-healing feature, ReplicaSet contribute to the overall resilience and high availability of applications in Kubernetes clusters.

**Manifest file for Replica-set**

{% highlight ruby%}
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: web-app
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web
        image: nginx
        ports:
        - containerPort: 80
  {% endhighlight %}
     


## kubernetes network

1. Kind- Service

- Service - In Kubernetes, a Service is a method for exposing a network application that is running as one or more Pods in our cluster.
- We use a Service to make that set of Pods available on the network so that clients can interact with it.

- Kubernetes Service types allow us to specify what kind of Service we want.The available type values and their behaviors are:-
1. Cluster-IP - Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default type that is used if we don't explicitly specify a type for a Service. This service type is used when we don't want to the
service to outside world

   {% highlight ruby %}
    apiVersion: v1
    kind: Service
    metadata:
      name: Backend
    spec:
      type: ClusterIP
      ports:
        - targetPort: 80
          port: 80
      selector:
        name: my-demo-pod 
   {% endhighlight %}

2. NodePort - NodePort service in Kubernetes is a service that is used to expose the application to the internet from where the end-users can access it.It will also expose the applications which are running in the node it also allows the traffic from the outside to reach the application with the help of NodePort.

- The NodePort port must be in the range between 30000 to 32767 and if we don’t specify the NodePort value Kubernetes will assign it randomly.
  

   {% highlight ruby %}
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: web-app
      spec:
        replicas: 3
        selector:
          matchLabels:
            app: web-containers
        template:
          metadata:
            labels:
              app: web-containers
          spec:
            containers:
            - image: nginx:latest
              name: web
              ports:
              - containerPort: 80
        ---
      apiVersion: v1
      kind: Service
      metadata:
        name: nginx-service
      spec:
        selector:
          app: web-containers
        ports:
        - port: 80
          protocol: TCP
          targetPort: 80
        type: NodePort
 
     {% endhighlight %}


 3. Load balancer -  A Kubernetes load balancer is a component that distributes network traffic across multiple instances of an application running in a K8S cluster. It acts as a traffic manager, ensuring that incoming requests are evenly distributed among the available instances to optimize performance and prevent overload on any single node, providing high availability and scalability.
 
-  The LoadBalancer service type is built on top of NodePort service types by provisioning and configuring external load balancers from cloud providers.
-   Load balancers in K8S can be implemented by using a cloud provider-specific load balancer such as Azure Load Balancer, AWS Network Load Balancer (NLB), or Elastic Load Balancer (ELB).

- Manifest file for service type - LoadBalancer
  
  {% highlight ruby %}
    apiVersion: v1
    kind: Service
    metadata:
      name: example-service
    spec:
      selector:
        app: example
      ports:
        - port: 8765
          targetPort: 9376
      type: LoadBalancer
   {% endhighlight %}

- Ad-hoc command to expose load balancer service
  **kubectl expose deployment example --port=8765 --target-port=9376 --name=example-service --type=LoadBalancer**











 





 