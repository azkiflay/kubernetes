- [Introduction](#introduction)
- [Terminology](#terminology)
- [Installation](#installation)
  - [Docker Installation](#docker-installation)
  - [VirtualBox Installation on Ubuntu 24.04 LTS](#virtualbox-installation-on-ubuntu-2404-lts)
  - [Minikube Installation](#minikube-installation)
- [References](#references)
# Introduction
Multiple Operating Systems (OSes) are able to run on a single server through virtualization solutions such as VMware, Xen, VirtualBox. Containerization tools (e.g., Docker) took hardware-level virtualization to the next level. Because containers provide OS-level virtualization, making application that run in containers to be self-contained. However, while containers solve problems, including package conflict and dependency, managing several containerized applications is not easy. While containers make it possible to deploy applications easily, managing so many of them created difficult. That's the where the need for container orchestration comes in, to create, deploy and manage thousands of containers. 

For example, [Docker Swam](https://docs.docker.com/engine/swarm/) is a container orchestration platform for Docker containers. Notably, [Kubernetes](https://kubernetes.io/) is one of the most widely used container orchestration platforms. Generally, there two planes in controller orchestration: **control plan** and **data plane**. An orchestrator, e.g., K8s, sits at the *control plane*, while containers, referred to as **worker nodes**, form the *data plane*. While containerized applications are actually run on the worker nodes, the controller administers them. 

Kubernetes orchestrates the deployment of containerized applications. It is an [open-source](https://github.com/kubernetes/kubernetes) software that has been widely used in cloud-native applications. Although originally developed by Google, Kubernetes is used across cloud service providers. Kubernetes is also referred to as **K8s** where the number 8 represents the number of characters between 'K' and 's' in the full name. K8s is the de facto platform for cloud-based container workloads.

# Terminology
* **Pod**: a number of containers that have the same Linux *namespace*, *cgroups*, *storage* and *network* resources.

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
* Download and install VirtualBox for Ubuntu/Debian from https://www.virtualbox.org/wiki/Downloads/. Download the VirtualBox *.deb* package for Ubuntu 20.04 if you would like to follow a similar environment as in this tutorial. For the purposes of this tutorial, the host machine is running Ubuntu 24.04 LTS, while the VM that will be created and managed will be Ubuntu 20.04. The reason for the later is because that is latest distribution available from Vagrant.
  ```bash
    #!/bin/bash
    sudo apt update
    sudo apt install -y build-essential dkms linux-headers-$(uname -r)
    sudo apt install virtualbox-7.1 # sudo apt remove virtualbox-7.1
    sudo apt --fix-broken install
    sudo dpkg -i ~/Downloads/virtualbox-7.1_7.1.12-169651~Ubuntu~focal_amd64.deb # TO ROMOVE: sudo apt remove virtualbox
    sudo apt install virtualbox-guest-additions-iso # TO REMOVE: sudo apt remove virtualbox-guest-additions-iso # If mismatch with Vagrant's expected version, 'vagrant up' won't work.
    vagrant plugin install vagrant-vbguest # On your host machine, install the vagrant-vbguest plugin
  ```

## Minikube Installation
```bash
    
```

# References
* Kubernetes documentation: https://kubernetes.io/docs/
* Kubernetes: Up and Running, 3rd Edition, by Brendan Burns, Joe Beda, Kelsey Hightower, and Lachlan Evenson (O’Reilly), 2022
* 
