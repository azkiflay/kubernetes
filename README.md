**Contents**
- [Introduction](#introduction)
- [Components of Kubernetes](#components-of-kubernetes)
- [Features of Kubernetes](#features-of-kubernetes)
- [Kubernetes Derivatives](#kubernetes-derivatives)
- [Installation](#installation)
  - [Docker Installation](#docker-installation)
  - [VirtualBox Installation on Ubuntu 24.04 LTS](#virtualbox-installation-on-ubuntu-2404-lts)
  - [Minikube Installation](#minikube-installation)
  - [Kubernetes Client (kubectl) Installation](#kubernetes-client-kubectl-installation)
  - [Installing K8s Dashboard](#installing-k8s-dashboard)
- [Tools and Terminology](#tools-and-terminology)
- [References](#references)
# Introduction
Multiple Operating Systems (OSes) are able to run on a single server through virtualization solutions such as VMware, Xen, VirtualBox. Containerization tools (e.g., Docker) took hardware-level virtualization to the next level. Because containers provide OS-level virtualization, making application that run in containers to be self-contained. However, while containers solve problems, including package conflict and dependency, managing several containerized applications is not easy. While containers make it possible to deploy applications easily, managing so many of them created difficult. That's the where the need for container orchestration comes in, to create, deploy and manage thousands of containers. 

For example, [Docker Swam](https://docs.docker.com/engine/swarm/) is a container orchestration platform for Docker containers. Notably, [Kubernetes](https://kubernetes.io/) is one of the most widely used container orchestration platforms. Generally, there two planes in controller orchestration: **control plan** and **data plane**. An orchestrator, e.g., K8s, sits at the *control plane*, while containers, referred to as **worker nodes**, form the *data plane*. While containerized applications are actually run on the worker nodes, the controller administers them. 

Kubernetes automates the deployment and management of containerized applications. It is an [open-source](https://github.com/kubernetes/kubernetes) software that has been widely used in cloud-native applications. Although originally developed by Google, Kubernetes is used across cloud service providers. Kubernetes is also referred to as **K8s** where the number 8 represents the number of characters between 'K' and 's' in the full name. K8s, pronounced as "*Kates*", is the de facto platform for cloud-based container workloads.

# Components of Kubernetes
- K8s is an OS for a cluster of computers.
- K8s provides the following OS-like services to applications:
  - service discovery: finding and accessing other applications' services
  - load balancing: sharing load among instances of the application.
  - self-healing: K8s restarts failed application copies
  - leader election: K8s decides which application copy is active at a given moment.
  - horizontal scaling: creating new containerized copies of the application.
- Computers in a K8s cluster belong to either **control plane** or **data plane**. The control plane consists of master nodes that control the cluster. For production, at least **three** master nodes to achieve high availability. Data plane is composed of computers, known as worker nodes, where actual applications are deployed. The number of nodes in the data plane can vary depending on the use case, but the size of the applications deployed through the K8s API should the worker nodes. For the user, K8s makes all worker nodes a single deployment surface. To manage the deployed application on them, worker nodes also run K8s components, which communicate with the K8s master nodes.
- **Control plane**:
  - **Scheduler**: allocates worker nodes the application instances each has to run.
  - **Controllers**: Create objects based on configuration defined by users.
  - **K8s API Server**: provides access to the cluster using RESTful API.
  - **etcd**: *persistent* data store for the API server, which has exclusive access to *etcd*. Although the API server is stateless, *etcd* persists objects created by the former.
- **Data plane**: 



# Features of Kubernetes
- **Abstraction of Infrastructure**: Users and application do not have to worry about specifics of hardware and network, as those are managed by K8s.
- **Standardized Deployment**: K8s enables application deployment across various cloud and on-premises environments using the same configuration file. Moreover, most cloud providers have adopted K8s, making it easy for users to deploy their application across hybrid-cloud infrastructure. Due to the wide adoption of K8s, users need not worry about compatibility or vendor lock-in. Because K8s APIs work across cloud service providers. Therefore, users do not necessarily have to deploy and manage their applications using proprietary APIs. Instead, they can use K8s APIs, which can transfer easily across major cloud providers such as Google Cloud, IBM Cloud, Microsoft Azure, and Amazon AWS.
- **Declarative Deployment**: K8s utilizes declarative model to create and monitor the health status of application components. When configuration of the deployed components is changed, K8s automatically achieves the desired state according to the new declaration. This includes changes made on purpose or when some component fails. In the latter, since K8s continuously takes actions so that the configuration of the desired state matches the actual state. If not, K8s takes necessary actions to remedy any failing or missing component. Consequently, K8s is a self-healing online system.
- **Application Management**: Once and application is deployed on it, K8s takes over its management without any manual involvement from users.
- **Application Scalability**: Due to its use of declarative configuration and immutable containers, K8s can easily scale up and down resources for applications deployed on it.
- **Microservice Management**: K8s automates the communication between microservices that are deployed across the infrastructure. 
- **Support for Devops**: As the infrastructure are abstracted by K8s, development and operations teams collaborate more easily. Moreover, K8s enables deployment of application by developers, minimizing their reliance on system administrators.
  
# Kubernetes Derivatives
Since K8s is an open-source system, commercial Kubernetes products have been derived from it, including [**Red Hat OpenShift**](https://www.redhat.com/en/technologies/cloud-computing/openshift) and [**Rancher**](https://www.rancher.com/).

<!-- # Note: EKS is not part of AWS free tier. EKS costs $0.10 per cluster per hour. So, resorting to **minikube** local Kubernetes with one controller node. Advanced concepts can be tried later on EKS for a fixed hour and with a clear execution plan, having mastered K8s skills on Minikube first.
# Amazon Elastic Kubernetes Service (EKS)
- On AWS. EKS is used to deploy and manage Kubernetes clusters. When using EKS, users spend more time on their specific use cases rather than on installing and maintaining Kubernetes.
- EKS is a managed service tha is used to run containerized applications. It reduces complexities of *networking*, *security*, *storage*, *scaling*, *load balancing*, and *observability*, and integration with other AWS services.
- In EKS, Amazon provides the control plane of the K8s, and the user attaches worker nodes to it.
- Self-managed Kubernetes cluster is an alternative to EKS on AWS. Of course, there are other similar solutions from other providers as well other than Amazon (e.g., Azure Kubernetes Service, Google Kubernetes Engine).
- Pre-requisite: AWS account, familiarity with Linux, Python, Terraform, YAML
- Tools and Interfaces: AWS CLI, eksctl, AWS CDK, Terraform, AWS Console, Helm
- EKS AMI images
-->

# Installation
## Docker Installation
```bash
    # Add Docker's official GPG key
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    # Add the repository to Apt sources
    echo \ "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    # Install the Docker packages
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    # Verify that the Docker Engine installation
    sudo docker run hello-world
```

## VirtualBox Installation on Ubuntu 24.04 LTS
<!-- TODO: Address redundancy by putting a link to github.com/azkiflay/ansible -->
* Download and install VirtualBox for Ubuntu/Debian from https://www.virtualbox.org/wiki/Downloads/. Download the VirtualBox *.deb* package for Ubuntu 20.04 if you would like to follow a similar environment as in this tutorial. For the purposes of this tutorial, the host machine is running Ubuntu 24.04 LTS.
  ```bash
    #!/bin/bash
    sudo apt update
    sudo apt install -y build-essential dkms linux-headers-$(uname -r)
    sudo apt install virtualbox-7.1 # TO ROMOVE: sudo apt remove virtualbox-7.1
    sudo apt --fix-broken install
    sudo dpkg -i ~/Downloads/virtualbox-7.1_7.1.12-169651~Ubuntu~focal_amd64.deb # TO ROMOVE: sudo apt remove virtualbox
    sudo apt install virtualbox-guest-additions-iso
    vagrant plugin install vagrant-vbguest
  ```
  
<!-- YOu can consider K8s in Docker (kind) as an alternative: https://kind.sigs.k8s.io/docs/user/quick-start-->
## Minikube Installation
```bash
    curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    sudo rm minikube-linux-amd64
    minikube start
```
Figure 1 shows a message that is displayed when starting Minikube following a successful installation.

<p align="center">
  <img src="figures/minikube_install_1.png" style="max-width:50%; height:auto;">
</p>
<p align="center"><strong>Figure 1:</strong> Starting Minikube</p>

## Kubernetes Client (kubectl) Installation
```bash
    # Download the latest kubectl release
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    # Download the checksum
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    # Verify the download kubectl against the checksum
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check # If valid, you should get "kubectl: OK". Otherwise, "kubectl: FAILED".
    # Install kubectl
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    # View version of installed kubectl
    kubectl version --client
    kubectl version --client --output=yaml
    rm kubectl # Remove the downloaded kubectl installerku
    rm kubectl.sha256 # Remove the downloaded kubectl checksum
```
Figure 2 displays the results of executing the above kubectl installation commands.

<p align="center">
  <img src="figures/kubectl_install_1.png" style="max-width:50%; height:auto;">
</p>
<p align="center"><strong>Figure 1:</strong> kubectl installation </p>

```bash
    kubectl get componentstatuses
    kubectl get nodes
    kubectl describe nodes minikube
    kubectl get deployments --namespace=kube-system coredns # K8s DNS
    # kubectl get services --namespace=kube-system coredns # TODO: load balancing for the DNS server. Error: "coredns" not found
```

## Installing K8s Dashboard
[Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) is a web-based interface for K8s. It can be used to deploy and manage cluster resources.
First, install [Helm](https://helm.sh/docs/intro/install/), which is the K8s package manager, as follows.
```bash
    curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
    sudo apt-get install apt-transport-https --yes
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
    sudo apt-get update
    sudo apt-get install helm

```

Subsequently, use **helm** to deploy the Dashboard User Interface (UI), as follow. Note that it is deployed by default when Minikube is installed.
```bash
    # Add kubernetes-dashboard repository
    helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
    # Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
    helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard # If kubernetes-dashboard-csrf exists error, first run "kubectl delete secret kubernetes-dashboard-csrf -n kubernetes-dashboard"
```

As can be seen in Figure 3, after the **helm** package manager finishes installing the Dashboard, a link to access it is given. In this case, that link is **https://localhost:8443**.

<p align="center">
  <img src="figures/dashboard_install_1.png" style="max-width:50%; height:auto;">
</p>
<p align="center"><strong>Figure 1:</strong> Dashboard installation </p>


# Tools and Terminology
* **Namespace**: an isolation and access control mechanism.
* **Pod**: a number of containers that have the same Linux *namespace*, *cgroups*, *storage* and *network* resources.
* **Service**:
* **Ingress**: A fronted exposed that can be accessed outside a K8s cluster.
* [**kubectl**](https://kubernetes.io/docs/tasks/tools/): is a command line tool for managing Kubernetes objects (e.g., Pods, Services, ReplicaSets)
* 
<!--
<figure>
<table>
  <tr>
    <td>
      <img src="figures/terraform_apply_3.png style="max-width:100%; height:auto;">
    </td>
    <td>
      <img src="figures/terraform_apply_4.png style="max-width:100%; height:auto;">
    </td>
  </tr>
</table>
<figcaption><strong>Figure 4: </strong> Modifying an existing instance </figcaption>
</figure>
-->

# References
* Kubernetes documentation: https://kubernetes.io/docs/
* Kubernetes: Up and Running, 3rd Edition, by Brendan Burns, Joe Beda, Kelsey Hightower, and Lachlan Evenson (O’Reilly), 2022
* Kubernetes in Action, 2nd Edition, by Marko Luksa, Manning, 2023
* 
