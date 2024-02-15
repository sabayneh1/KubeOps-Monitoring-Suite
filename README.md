Kubernetes Monitoring Suite with Prometheus and Grafana

This project provides a comprehensive suite for monitoring Kubernetes clusters, leveraging the power of Prometheus for metrics collection and Grafana for metrics visualization. It includes Kubernetes deployment configurations, DaemonSet setups, and instructions for setting up Prometheus and Grafana within your Kubernetes cluster.

Features

Kubernetes Deployments: Example configurations for deploying applications within a Kubernetes cluster.
DaemonSet for Node Labeling: A DaemonSet configuration to apply custom labels to your nodes for advanced scheduling.
Prometheus Monitoring: Setup instructions for deploying Prometheus in a Kubernetes cluster to collect metrics.
Grafana Dashboards: Steps to integrate Grafana with Prometheus for visualizing metrics in detailed dashboards.

Prerequisites

A Kubernetes cluster
kubectl configured to communicate with your cluster
Helm 3

Setup Instructions

1. Installing Helm
Helm is a package manager for Kubernetes that facilitates the installation and management of applications.


curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
alias helm="sudo helm"

2. Deploying Prometheus and Grafana
3. 
Prometheus:

Follow the instructions provided by AWS EKS to deploy Prometheus in your cluster:
Deploying Prometheus on Amazon EKS

Grafana:

Set up Grafana dashboards to visualize metrics from Prometheus using the guide below:
Setting Up Grafana Dashboards

3. Deploying Application and DaemonSet
Apply the YAML configurations for your applications and the node labeler DaemonSet:


kubectl apply -f app-a.yaml
kubectl apply -f app-b.yaml
kubectl apply -f Nodelabler.yaml
# Add any additional configurations as needed


4. Managing Deployments
   
To scale, edit, or delete deployments, use the following kubectl commands:

# Scale deployment app-a to 4 replicas
kubectl scale deployment app-a --replicas=4

# Edit Prometheus service

kubectl edit svc prometheus-server -n prometheus

# Delete deployment

kubectl delete deployment app-a

5. Viewing Resources
Utilize kubectl commands to view deployments, services, and other resources:

kubectl get deployments
kubectl get svc -n prometheus
kubectl get pods -o wide
kubectl get nodes
kubectl describe node <node-name> | grep failure-domain.beta.kubernetes.io/zone


Monitoring and Observability

After setting up Prometheus and Grafana, you can access Grafana's dashboards to visualize the metrics collected from your Kubernetes cluster. This project includes example dashboards configured to display a variety of metrics, including CPU usage, memory consumption, and more.

Conclusion

This Kubernetes Monitoring Suite is designed to offer a robust solution for monitoring Kubernetes clusters efficiently. By leveraging Prometheus for metrics collection and Grafana for visualization, it provides the tools necessary for maintaining the health and performance of your Kubernetes applications.

For more detailed instructions and configuration options, refer to the official documentation of each tool.
