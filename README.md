Kubernetes Monitoring Suite with Prometheus and Grafana on AWS EKS

Description

Enhance your AWS EKS Kubernetes clusters' monitoring capabilities with this robust suite utilizing Prometheus for detailed metrics collection and Grafana for interactive, insightful dashboards. This guide covers everything from setup to advanced configuration, including metrics collection and dashboard customization.

Main Features

  Pre-configured Kubernetes Manifests: Quick-start your deployments.
  Streamlined Prometheus and Grafana Installation: Easy setup with Helm 3.
  Application Instrumentation Guidance: Implement RED metrics and more.
  Grafana Dashboards Examples: Visualize metrics out of the box.
  Extensive Documentation: For customization and advanced scenarios.

Prerequisites

AWS EKS cluster up and running.
kubectl configured for your cluster.
Helm 3 installed locally.

Metric Server Requirement

For Prometheus to function correctly, a metric server is necessary as it exposes a new API through the API server. This can be accessed at metrics.k8s.io for detailed metrics information. Install jq and retrieve metrics API data with:
