- [Introduction](#introduction)
- [Installation](#installation)
  - [Docker Installation](#docker-installation)
- [References](#references)
# Introduction
Multiple Operating Systems (OSes) are able to run on a single server through virtualization solutions such as VMware, Xen, VirtualBox. Containerization tools (e.g., Docker) took hardware-level virtualization to the next level. Containers provide OS-level virtualization, making application that run in containers to be self-contained. However, while containers solve problems, including package conflict and dependency, managing several containerized applications is not easy. That's where Kubernetes comes it.

Kubernetes orchestrates the deployment of containerized applications. It is an [open-source](https://github.com/kubernetes/kubernetes) software that has been widely used in cloud-native applications. Although originally developed by Google, Kubernetes is used across cloud service providers.

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

# References
* Kubernetes documentation: https://kubernetes.io/docs/
* Kubernetes: Up and Running, 3rd Edition, by Brendan Burns, Joe Beda, Kelsey Hightower, and Lachlan Evenson (O’Reilly), 2022
* 
