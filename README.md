# **Kubernetes Monitoring Suite with Prometheus and Grafana on AWS EKS**

## **Description**

This project equips you with a full-fledged suite for monitoring Kubernetes clusters on AWS EKS, employing Prometheus for detailed metrics collection and Grafana for dynamic visualizations. It offers out-of-the-box deployment configurations, comprehensive guides for Prometheus and Grafana setup, and advice on instrumenting your applications to track RED metrics (Request Rate, Error Rate, and Duration) among other essential metrics.

## **Main Features**

- **Pre-configured Kubernetes Manifests:** Streamline your deployments with our ready-made configurations.
- **Effortless Prometheus and Grafana Installation:** Follow our Helm 3 based guide for quick setup on AWS EKS.
- **Application Instrumentation Insights:** Implement RED metrics and key performance indicators in your applications.
- **Grafana Dashboard Examples:** Jumpstart your monitoring with our dashboard templates.
- **Extensive Documentation:** Dive deeper with our guides for customization and advanced features.

## **Prerequisites**

- An active AWS EKS cluster.
- **`kubectl`** setup for your EKS cluster.
- Helm 3 installed on your machine.

## **Metric Server Requirement**

Before deploying Prometheus, ensure the Metric Server is installed in your cluster as it provides essential APIs for metrics collection.

- **Install jq and View Metrics API:**

```bash
sudo snap install jq
kubectl get --raw /apis/metrics.k8s.io/v1beta1 | jq "."

```

## **Getting Started**

### **1. Install Helm 3**

Helm simplifies the management of Kubernetes applications. Install it with:

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
alias helm="sudo helm"
```

### **Deploy Prometheus**

- **Add Prometheus Helm Repository:**

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

- **Download and Deploy Prometheus:**

```bash
helm pull prometheus-community/prometheus
helm install prometheus prometheus-community/prometheus -f values-aws-eks.yaml
```

- **Access Prometheus Dashboard:**

```bash
kubectl get svc prometheus-server -o jsonpath='{.status.nodePort}'
```

### **Deploy Grafana**

- **Add Grafana Helm Repository:**

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

- **Deploy Grafana:**

```bash
helm install grafana grafana/grafana -f values-aws-eks.yaml
```

- **Access Grafana Dashboard:**

```bash
kubectl get svc grafana -o jsonpath='{.status.nodePort}'

```

### **Grafana Login Credentials**

Retrieve Grafana's admin password:

```bash
kubectl get secret grafana -o jsonpath='{.data}'
```

Decode the password (e.g., if username is "YWRtaW4="):

```bash
echo "YWRtaW4=" | base64 --decode
```

### **Integrate Prometheus with Grafana**

- **Add Prometheus as a Data Source in Grafana:** Navigate to Grafana's Data Sources, add a new source, and input the URL of your Prometheus server.
- **Importing Grafana Dashboards:** Select dashboards from [Grafana Dashboards](https://grafana.com/grafana/dashboards), copy the ID of your chosen dashboard, then go to Grafana's Dashboard Import section and paste the ID to import.

### **3. Deploying Applications and DaemonSet**

```bash
kubectl apply -f app-a.yaml
kubectl apply -f app-b.yaml
kubectl apply -f Nodelabler.yaml
```

### **4. Managing Deployments**

Manage your deployments with **`kubectl`**:

```bash
kubectl scale deployment app-a --replicas=4
kubectl edit svc prometheus-server -n prometheus
kubectl delete deployment app-a
```

### **5. Viewing Kubernetes Resources**

```bash
kubectl get deployments
kubectl get svc -n prometheus
kubectl get pods -o wide
kubectl get nodes
kubectl describe node <node-name>
```

## **Monitoring and Observability**

With Prometheus and Grafana set up, you can visualize a wide range of metrics from your Kubernetes cluster, including but not limited to CPU usage and memory consumption.
