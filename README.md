# prometheus_project
# Monitoring Kubernetes Cluster Using Prometheus And Grafana

This repository offers comprehensive instructions for setting up a monitoring infrastructure for Kubernetes clusters using Prometheus and Grafana. By following the steps outlined below, users can establish a robust monitoring system to track the health, performance, and resource utilization of their Kubernetes environments. Additionally, we implement service discovery within Prometheus to automatically detect and integrate new nodes into the monitoring system, ensuring seamless scalability and comprehensive coverage of the Kubernetes cluster.

## Table Of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Conclusion](#conclusion)
4. [Outputs](#outputs)


## Prerequisites

- *Docker*
- *Minikube*
- *Helm*
- *Prometheus*
- *Grafana*

## Installation

## Docker Installation

First you have to uninstall old versions. 
```bash
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

Then begin the installation process:

1. Install `` yum-utils`` package:
```bash 
$ sudo yum install -y yum-utils 
```

2. Add `docker-ce.repo` repository to download Docker:
```bash 
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
```
3. Installing Docker packages:
```bash 
$ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
```
4. Enabling Docker service:
```bash 
$ sudo systemctl enable --now docker
```

## Install and Set Up kubectl

1. Download the latest release with the command:
```bash 
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```

2. Make the kubectl binary executable:
```bash 
$ chmod +x ./kubectl
```

3. Move the binary into your `PATH`:
```bash 
$ sudo mv ./kubectl /usr/local/bin/kubectl
```

4. Verifying Installation:
```bash 
$ kubectl version
```

## Minikube Installation

1. Download Minikube last release:
```bash 
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
2. Install Minikube package:
```bash 
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
3. Start Minikube:
```bash 
$ minikube start --driver=docker --force
```

## Helm Installation

1. Fetch that script:
```bash 
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
2. Provide execute permission to the file:
```bash 
$ chmod 700 get_helm.sh
```
3. Run the script:
```bash 
$ ./get_helm.sh
```

### Deploy Prometheus
1.Add Prometheus Helm repository
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
2.Update the Helm repositories:
```bash
helm repo update 
```
3.Install Prometheus using Helm:
```bash
helm install prometheus prometheus-community/prometheus
```

4.Expose Prometheus service:
```bash
kubectl expose service prometheus-server — type=NodePort — target-port=9090 — name=prometheus-server-ext
```
5.Open Web App of Prometheus
```bash
minikube service prometheus-server-ext
```


### Deploy Grafana

1.Add Grafana Helm repository:
```bash
helm repo add grafana https://grafana.github.io/helm-charts
```
2.Update the Helm repositories:
```bash
helm repo update 
```
3.Install Grafana using Helm:
```bash
helm install grafana grafana/grafana
```
3.Expose Grafana service:
```bash
kubectl expose service grafana — type=NodePort — target-port=3000 — name=grafana-ext
```
3.Open Grafana Web APP
```bash
minikube service grafana-ext
```

## Get Grafana Credintials
```bash
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
### Conclusion
In summary, this repository presents a guide for implementing a monitoring infrastructure tailored for Kubernetes clusters, leveraging Prometheus and Grafana. By following the steps provided, users can establish a robust monitoring system to oversee the health, performance, and resource utilization of their Kubernetes environments. Additionally, the integration of service discovery within Prometheus ensures effortless scalability and seamless integration of new nodes into the monitoring ecosystem. With these tools at hand, users can confidently monitor and manage their Kubernetes clusters effectively.


## Outputs



![k dashboard](https://github.com/RowanHomam/prometheus_project/assets/161208993/094a5f15-abd0-4af8-a9b5-9e49bea025c0)
![prometheus](https://github.com/RowanHomam/prometheus_project/assets/161208993/2b6a3b98-7351-4bd6-a644-3965c71ab246)
