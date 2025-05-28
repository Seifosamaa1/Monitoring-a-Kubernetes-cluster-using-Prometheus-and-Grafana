# Monitoring-a-Kubernetes-cluster-using-Prometheus-and-Grafana
We're setting up a Kubernetes cluster using Minikube. Then, integrating Prometheus and Grafana to monitor the health and performance of our containers.
prometheus_project
Monitoring Kubernetes Cluster Using Prometheus And Grafana
This repository offers comprehensive instructions for setting up a monitoring infrastructure for Kubernetes clusters using Prometheus and Grafana. By following the steps outlined below, users can establish a robust monitoring system to track the health, performance, and resource utilization of their Kubernetes environments. Additionally, we implement service discovery within Prometheus to automatically detect and integrate new nodes into the monitoring system, ensuring seamless scalability and comprehensive coverage of the Kubernetes cluster.

Table Of Contents
Prerequisites
Installation
Conclusion
Outputs
Prerequisites
Docker
Minikube
Helm
Prometheus
Grafana
Installation
Docker Installation
First you have to uninstall old versions.

$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
Then begin the installation process:

Install  yum-utils package:
$ sudo yum install -y yum-utils 
Add docker-ce.repo repository to download Docker:
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
Installing Docker packages:
$ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
Enabling Docker service:
$ sudo systemctl enable --now docker
Install and Set Up kubectl
Download the latest release with the command:
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
Make the kubectl binary executable:
$ chmod +x ./kubectl
Move the binary into your PATH:
$ sudo mv ./kubectl /usr/local/bin/kubectl
Verifying Installation:
$ kubectl version
Minikube Installation
Download Minikube last release:
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
Install Minikube package:
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
Start Minikube:
$ minikube start --driver=docker --force
Helm Installation
Fetch that script:
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
Provide execute permission to the file:
$ chmod 700 get_helm.sh
Run the script:
$ ./get_helm.sh
Deploy Prometheus
1.Add Prometheus Helm repository

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
2.Update the Helm repositories:

helm repo update 
3.Install Prometheus using Helm:

helm install prometheus prometheus-community/prometheus
4.Expose Prometheus service:

kubectl expose service prometheus-server — type=NodePort — target-port=9090 — name=prometheus-server-ext
5.Open Web App of Prometheus

minikube service prometheus-server-ext
Deploy Grafana
1.Add Grafana Helm repository:

helm repo add grafana https://grafana.github.io/helm-charts
2.Update the Helm repositories:

helm repo update 
3.Install Grafana using Helm:

helm install grafana grafana/grafana
3.Expose Grafana service:

kubectl expose service grafana — type=NodePort — target-port=3000 — name=grafana-ext
3.Open Grafana Web APP

minikube service grafana-ext
Get Grafana Credintials
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
Conclusion
In summary, this repository presents a guide for implementing a monitoring infrastructure tailored for Kubernetes clusters, leveraging Prometheus and Grafana. By following the steps provided, users can establish a robust monitoring system to oversee the health, performance, and resource utilization of their Kubernetes environments. Additionally, the integration of service discovery within Prometheus ensures effortless scalability and seamless integration of new nodes into the monitoring ecosystem. With these tools at hand, users can confidently monitor and manage their Kubernetes clusters effectively. make this good for me to make it in readme in github 
