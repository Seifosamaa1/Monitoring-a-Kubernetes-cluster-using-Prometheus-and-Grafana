📊 Monitoring Kubernetes Cluster Using Prometheus and Grafana
This repository provides comprehensive, step-by-step instructions for setting up a robust monitoring infrastructure for Kubernetes clusters using Prometheus and Grafana.

By following this guide, you'll be able to:

Monitor cluster health, performance, and resource usage.

Automatically discover and integrate new nodes using Prometheus service discovery.

Visualize metrics through Grafana dashboards.

📚 Table of Contents
Prerequisites

Installation

Docker

kubectl

Minikube

Helm

Prometheus

Grafana

Outputs

Conclusion

🔧 Prerequisites
Ensure the following tools are installed before proceeding:

Docker

Minikube

Helm

Prometheus

Grafana

⚙️ Installation
Docker Installation
Remove old Docker versions:

bash
Copy
Edit
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
Install dependencies and Docker:

bash
Copy
Edit
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
Install and Set Up kubectl
bash
Copy
Edit
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
Minikube Installation
bash
Copy
Edit
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=docker --force
Helm Installation
bash
Copy
Edit
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
Deploy Prometheus
Add Prometheus Helm repo:

bash
Copy
Edit
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
Install Prometheus:

bash
Copy
Edit
helm install prometheus prometheus-community/prometheus
Expose Prometheus service:

bash
Copy
Edit
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
Access Prometheus Web UI:

bash
Copy
Edit
minikube service prometheus-server-ext
Deploy Grafana
Add Grafana Helm repo:

bash
Copy
Edit
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
Install Grafana:

bash
Copy
Edit
helm install grafana grafana/grafana
Expose Grafana service:

bash
Copy
Edit
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
Access Grafana Web UI:

bash
Copy
Edit
minikube service grafana-ext
Retrieve Grafana admin password:

bash
Copy
Edit
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
📤 Outputs
Once setup is complete:

Prometheus will be available at the exposed NodePort.

Grafana dashboards will be accessible and ready to visualize metrics.

New nodes/services in your Kubernetes cluster will automatically be discovered.

✅ Conclusion
This project offers a complete guide for establishing a monitoring infrastructure for Kubernetes using Prometheus and Grafana. With built-in service discovery and intuitive dashboards, you’ll gain real-time insights into your cluster’s performance, helping you ensure stability, scalability, and visibility.
