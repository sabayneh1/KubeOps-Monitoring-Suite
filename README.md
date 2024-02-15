# **Kubernetes Monitoring Suite with Prometheus and Grafana on AWS EKS**

## **Description**

Dive deeper into our comprehensive monitoring suite designed for Kubernetes clusters on AWS EKS, now with enhanced deployment strategies, high availability configurations, and security practices. Beyond the core capabilities of leveraging Prometheus for detailed metrics collection and Grafana for insightful visual dashboards, this suite introduces advanced deployment features. These include pod anti-affinity and node affinity settings to ensure pods are evenly distributed across different nodes, enhancing the resilience and availability of your applications. Additionally, the suite showcases a DaemonSet named node-labeler, exemplifying how to automatically run a pod on every node and label them based on specific criteria—a key strategy for efficient cluster and workload management. The integration of environment variables and volume mounts further demonstrates how to tailor pod configurations to interact seamlessly with the underlying environment and AWS services. Moreover, with a focus on security, this suite presents a network policy tailored for the Prometheus server, restricting ingress traffic to essential ports and showcasing your command over Kubernetes network policies for safeguarding your applications within the cluster.

## **Main Features**

- **Advanced Deployment Configurations**: Leverage Kubernetes deployment manifests featuring pod anti-affinity and node affinity to ensure distributed pod placement for enhanced high availability.
- **Node-Labeler DaemonSet**: Automatically label nodes across your cluster with the node-labeler DaemonSet, optimizing cluster management and workload distribution.
- **Sophisticated Environment Configuration**: Utilize environment variables and volume mounts for dynamic pod configuration, enabling robust interaction with AWS services.
- **Enhanced Security with Network Policies**: Secure your monitoring infrastructure with custom network policies for Prometheus, ensuring controlled access to critical services.
- **Comprehensive Monitoring with RED Metrics**: Get started with pre-configured dashboards focusing on RED metrics—Request Rate, Error Rate, and Duration—providing a solid foundation for monitoring application performance and identifying issues promptly.

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
## **Implementing RED Metrics and Key Performance Indicators**

Understanding and monitoring RED metrics—Request Rate, Error Rate, and Duration—is crucial for effectively managing the performance and reliability of your applications. Prometheus and Grafana offer the tools needed to collect, visualize, and analyze these metrics, providing insights into how well your applications are performing and where improvements are needed.

### **Advice on RED Metrics:**

- **Request Rate**: Monitor the number of requests your application is handling. This can help you understand the demand on your services and plan for scaling.
- **Error Rate**: Keep a close eye on the rate of failed requests. A high error rate may indicate underlying problems with your application or infrastructure that require attention.
- **Duration**: Track the response time of your requests. Longer durations may suggest performance bottlenecks or capacity issues that need optimization.

### **Setting Up Key Performance Indicators in Prometheus and Grafana:**

1. **Configure Prometheus** to collect metrics related to HTTP requests, errors, and response times from your applications.
2. **Use Grafana** to create dashboards that visualize these metrics in real-time. This will allow you to quickly assess the health and performance of your services.
3. **Establish Alerts** within Grafana based on thresholds for these metrics. For instance, set alerts for when the error rate exceeds a certain percentage or when response times spike unexpectedly.

Incorporating these practices into your monitoring strategy will enable you to maintain a high level of service reliability and performance, ensuring a positive experience for your users.

## **Monitoring and Observability**
With Prometheus and Grafana set up, you can visualize a wide range of metrics from your Kubernetes cluster, including but not limited to CPU usage and memory consumption.
